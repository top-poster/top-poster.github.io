---
layout: post
title: "AWS Cognito 및 Amply를 사용하여 React 앱에 인증 추가"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2020/02/authentication-react-apps-aws-amplify-cognito.jpeg
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/02/authentication-react-apps-aws-amplify-cognito.jpeg?fit=730%2C486&ssl=1)

이 튜토리얼에서는 AWS Cognito를 사용한 React 앱과 Amply 프레임워크를 사용한 인증에 대해 설명할 것이다. 먼저 인증의 기본 사항 및 각 AWS 솔루션이 React 앱을 안정적으로 인증하는 데 어떻게 도움이 되는지 살펴보겠습니다.

## AWS Cognito 소개 및 확장

인증은 전자 상거래, 게임, 물류 또는 다른 분야에 관계없이 새로운 프로젝트를 시작할 때 가장 먼저 검토해야 하는 사항 중 하나입니다.

특정 조직에는 재사용할 수 있는 기존 솔루션이 있을 수 있습니다. 하지만 운이 좋지 않다면 직접 구현하거나 기존 솔루션을 기반으로 구축하도록 선택할 수 있습니다. 이는 매우 권장되며 많은 시간을 절약합니다.

본 튜토리얼에서는 AWS Cognito 사용자 풀 및 Amply Framework를 사용하여 향후 및 일부 Retact 앱에 인증을 추가하는 방법에 대해 설명합니다.

### AWS Cognito 사용자 풀이란?

문서에 정의된 대로 Amazon Cognito 사용자 풀은 사용자 등록, 인증 및 계정 복구를 처리하는 모든 기능을 갖춘 사용자 디렉토리 서비스입니다.

사용자 풀은 Amazon Cognito에 있는 사용자 디렉터리입니다. 사용자 풀을 사용하면 사용자가 Amazon Cognito를 통해 웹 또는 모바일 앱에 로그인할 수 있습니다. 또한 사용자는 Facebook이나 Amazon과 같은 소셜 ID 제공자와 SAML ID 제공자를 통해 로그인할 수 있습니다.

사용자가 직접 로그인하든 제3자를 통해 로그인하든 사용자 풀의 모든 구성원은 사용자가 SDK를 통해 액세스할 수 있는 디렉토리 프로파일을 가집니다.

### 증폭 프레임워크란 무엇입니까?

Amply Framework는 AWS의 유연하고 확장 가능하며 안정적인 서버리스 백엔드에 정교한 클라우드 기반 애플리케이션을 구축하기 위한 포괄적인 라이브러리입니다. Amply를 사용하면 AWS에서 제공하는 클라우드 서비스 어레이에 액세스할 수 있습니다.

## CLI 설정 확대

Amply에 액세스하려면 AWS 계정이 있어야 합니다. 이미 사용 중인 AWS 무료 티어가 있으면 바로 이용할 수 있습니다. 없는 경우 AWS 무료 티어에 등록할 수 있습니다.

CLI를 설치하려면 다음 명령을 실행합니다.

```coffeescript
$ npm install -g @aws-amplify/cli
```

설치가 완료되면 다음을 실행하여 CLI를 구성할 수 있습니다.

```shell
$ amplify configure
```

그러면 AWS 계정에 로그인하고 사용자 이름을 선택하고 새 관리자를 설정한 다음 `비밀 액세스 키`와 `액세스 키 ID`를 생성하는 일련의 설명되고 간단한 단계를 거치게 됩니다. 이 키는 `~/aws/credentials`에 구성된 AWS 프로필에 저장됩니다.

설치에 성공하면 새 사용자가 성공적으로 설정되었음을 확인할 수 있습니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/02/new-user-setup-successful.png?resize=730%2C178&ssl=1)

## 반응 앱 설정

모든 프로젝트에 대해 TypeScript를 사용하는 것이 좋기 때문에 새로운 TypeScript/React 프로젝트를 시작하기 위해 다음 명령을 실행하여 앱을 부트스트랩할 수 있습니다.

```shell
$ create-react-app react-amplify-example --typescript && cd react-amplify-example
```

부트스트랩은 앱이 완성되는 데 몇 분 정도 걸리므로, 조급해 질 경우를 대비해 커피 한 잔을 챙길 수 있다. 🙂

Amply 프로젝트를 시작하기 위해 다음 명령을 실행하여 프로젝트를 초기화하고 구성합니다.

```shell
$ amplify init
```

그러면 프로젝트에 가장 적합한 설정을 선택하는 옵션 단계가 실행됩니다. 이 경우 다음과 같은 옵션을 사용할 수 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/02/project-settings.png?resize=730%2C189&ssl=1)

배포가 즉시 시작되고, 그 후에 다음 내용과 일치하는 성공 메시지가 표시됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/02/deployment-successful.png?resize=730%2C294&ssl=1)

배포가 완료되면 생성된 `aws-export.js` 파일이 `src` 폴더에 나타납니다. 변경 내용을 배포할 때마다 변경되므로 이 파일을 수정하면 안 됩니다.

다음으로, 인증 리소스를 앱에 추가해야 합니다. 다음 명령을 실행하여 Cognito 풀에 대한 구성 옵션을 선택하십시오.

```shell
$ amplify add auth
```

앱에 가장 적합한 구성 옵션을 사용하려면 수동 구성을 선택하고 메뉴에서 다음 옵션을 선택하십시오.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/02/auth-config-options.png?resize=730%2C264&ssl=1)

암호 구성 옵션이 미리 보기에 표시되지 않으므로 계속하여 아래 이미지를 첨부했습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/02/password-config-options.png?resize=730%2C315&ssl=1)

클라우드에 새 리소스 변경 사항을 배포하려면 다음을 실행하십시오.

```shell
$ amplify push
```

이제 Amply 및 Cognito 설정이 완료되었으며 종속성을 설치할 수 있습니다. 이렇게 하려면 다음 명령을 실행합니다.

```shell
$ yarn add aws-amplify react-router-dom styled-components antd password-validator jwt-decode
```

## 데모: AWS Cognito 및 Amply 작업

다음은 샘플 앱의 소스 코드로 넘어가겠습니다. 파일의 로직을 분해하면서 하나씩 파일 주소를 지정할 것입니다.

### 'SignUp Container.tsx'

이 구성 요소에는 새 사용자를 등록하는 데 필요한 논리가 포함되어 있습니다. UI 구성 요소에는 Ant Design을 사용할 것이며, 사용자 암호의 유효성을 확인하기 위해 암호 검증기를 사용할 것입니다. 사용자 풀을 설정하는 동안 암호 요구 사항을 추가했습니다. 암호 요구 사항은 다음과 같습니다.

- 최소 8자
- 대문자가 하나 이상 있습니다.
- 소문자가 하나 이상 있습니다.
- 하나 이상의 기호가 있음
- 숫자를 하나 이상 가지고 있습니다.

필요한 모든 사용자 세부 정보를 성공적으로 검증한 후, 우리는 페이로드를 `Cognito API`로 전송하여 사용자의 이메일로 확인 코드를 전송하고 `UserPool`에 새로운 사용자를 생성한다. 코드는 다음과 같습니다.

```coffeescript
import * as React from "react";
import { Link, Redirect } from 'react-router-dom';
import { Auth } from 'aws-amplify';
import { Form, Input, Icon, Button, notification, Popover, Spin, Col, Row } from 'antd';

// Presentational 
import FormWrapper from '../../Components/Styled/FormWrapper';

// App theme 
import { colors } from '../../Themes/Colors';

type Props = {
  form: any;
};

type State = {
  confirmDirty: boolean;
  redirect: boolean;
  loading: boolean;
  email: string;
};

type UserFormData = {
  fname: string;
  lname: string;
  password: string;
  email: string;
  phoneNumber: number;
};

const passwordValidator = require('password-validator');

// create a password schema
const schema = new passwordValidator();

schema
  .is()
  .min(8)
  .has()
  .uppercase()
  .has()
  .lowercase()
  .has()
  .digits()
  .has()
  .symbols();

class SignUpContainer extends React.Component<Props, State> {
  state = {
    confirmDirty: false,
    redirect: false,
    loading: false,
    email: ''
  };

  handleSubmit = (event: React.FormEvent) => {
    event.preventDefault();

    this.props.form.validateFieldsAndScroll((err: Error, values: UserFormData) => {
      if (!err) {
        let { fname, lname, password, email, phoneNumber } = values;

        // show loader
        this.setState({ loading: true });

        Auth.signUp({
          username: email,
          password,
          attributes: {
            email,
            name: ${fname} ${lname},
            phone_number: phoneNumber
          }
        })
          .then(() => {
            notification.success({
              message: 'Succesfully signed up user!',
              description: 'Account created successfully, Redirecting you in a few!',
              placement: 'topRight',
              duration: 1.5,
              onClose: () => {
                this.setState({ redirect: true });
              }
            });

            this.setState({ email });
          })
          .catch(err => {
            notification.error({
              message: 'Error',
              description: 'Error signing up user',
              placement: 'topRight',
              duration: 1.5
            });

            this.setState({
              loading: false
            });
          });
      }
    });
  };

  handleConfirmBlur = (event: React.FormEvent<HTMLInputElement>) => {
    const { value } = event.currentTarget;

    this.setState({ confirmDirty: this.state.confirmDirty || !!value });
  };

  compareToFirstPassword = (rule: object, value: string, callback: (message?: string) => void) => {
    const { form } = this.props;

    if (value && value !== form.getFieldValue('password')) {
      callback('Two passwords that you enter is inconsistent!');
    } else {
      callback();
    }
  };

  validateToNextPassword = (rule: object, value: string, callback: (message?: string) => void) => {
    const form = this.props.form;
    const validationRulesErrors = schema.validate(value, { list: true });

    if (value && this.state.confirmDirty) {
      form.validateFields(['confirm'], { force: true });
    }
    if (validationRulesErrors.length > 0) {
      callback(this.formatPasswordValidateError(validationRulesErrors));
    }
    callback();
  };

  formatPasswordValidateError = (errors: Array<string>) => {
    for (let i = 0; i < errors.length; i++) {
      if (errors[i] === 'min') {
        return 'password length should be a at least 8 characters';
      } else if (errors[i] === 'lowercase') {
        return 'password should contain lowercase letters';
      } else if (errors[i] === 'uppercase') {
        return 'password should contain uppercase letters';
      } else if (errors[i] === 'digits') {
        return 'password should contain digits';
      } else if (errors[i] === 'symbols') {
        return 'password should contain symbols';
      }
    }
  };

  render() {
    const { getFieldDecorator } = this.props.form;
    const { redirect, loading } = this.state;

    const title = 'Password Policy';
    const passwordPolicyContent = (
      <React.Fragment>
        <h4>Your password should contain: </h4>
        <ul>
          <li>Minimum length of 8 characters</li>
          <li>Numerical characters (0-9)</li>
          <li>Special characters</li>
          <li>Uppercase letter</li>
          <li>Lowercase letter</li>
        </ul>
      </React.Fragment>
    );

    return (
      <React.Fragment>
        <FormWrapper onSubmit={this.handleSubmit}>
          <Form.Item>
            {getFieldDecorator('fname', {
              rules: [
                {
                  required: true,
                  message: 'Please input your first name!'
                }
              ]
            })(
              <Input
                prefix={<Icon type="user" style={ color: colors.transparentBlack } />}
                placeholder="First Name"
              />
            )}
          </Form.Item>
          <Form.Item>
            {getFieldDecorator('lname', {
              rules: [
                {
                  required: true,
                  message: 'Please input your last name!'
                }
              ]
            })(
              <Input prefix={<Icon type="user" style={ color: colors.transparentBlack } />} placeholder="Last Name" />
            )}
          </Form.Item>
          <Form.Item>
            {getFieldDecorator('email', {
              rules: [{ required: true, message: 'Please input your email!' }]
            })(<Input prefix={<Icon type="user" style={ color: colors.transparentBlack } />} placeholder="Email" />)}
          </Form.Item>
          <Form.Item>
            {getFieldDecorator('phoneNumber', {
              rules: [
                {
                  required: true,
                  message: 'Please input your phone number!'
                }
              ]
            })(
              <Input
                prefix={<Icon type="phone" style={ color: colors.transparentBlack } />}
                placeholder="Phone Number"
              />
            )}
          </Form.Item>
          <Form.Item>
            <Popover placement="right" title={title} content={passwordPolicyContent} trigger="focus">
              {getFieldDecorator('password', {
                rules: [
                  { required: true, message: 'Please input your Password!' },
                  {
                    validator: this.validateToNextPassword
                  }
                ]
              })(
                <Input
                  prefix={<Icon type="lock" style={ color: colors.transparentBlack } />}
                  type="password"
                  placeholder="Password"
                />
              )}
            </Popover>
          </Form.Item>
          <Form.Item>
            {getFieldDecorator('confirm', {
              rules: [
                {
                  required: true,
                  message: 'Please confirm your password!'
                },
                {
                  validator: this.compareToFirstPassword
                }
              ]
            })(
              <Input
                prefix={<Icon type="lock" style={ color: colors.transparentBlack } />}
                type="password"
                placeholder="Confirm Password"
                onBlur={this.handleConfirmBlur}
              />
            )}
          </Form.Item>

          <Form.Item className="text-center">
            <Row>
              <Col lg={24}>
                <Button style={ width: '100%' } type="primary" disabled={loading} htmlType="submit">
                  {loading ? <Spin indicator={<Icon type="loading" style={ fontSize: 24 } spin />} /> : 'Register'}
                </Button>
              </Col>
              <Col lg={24}>
                Or <Link to="/login">login to your account!</Link>
              </Col>
            </Row>
          </Form.Item>
        </FormWrapper>
        {redirect && (
          <Redirect
            to={
              pathname: '/verify-code',
              search: ?email=${this.state.email}
            }
          />
        )}
      </React.Fragment>
    );
  }
}

export default Form.create()(SignUpContainer);
```

이제 이와 유사한 기능을 사용할 수 있습니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/02/signup-container.png?resize=651%2C625&ssl=1)

등록 후 사용자 풀에는 다음과 같은 새 사용자 목록이 있어야 합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/02/app-user-pool.png?resize=730%2C187&ssl=1)

### 'ConfirmEmailContainer.tsx' 추가

등록에 성공하면 확인 코드가 사용자의 전자 메일로 전송됩니다. 이를 확인하려면 다음 코드를 추가하십시오. 제출 단추와 함께 기본 입력이 있습니다.

확인 코드가 제출되면, 우리는 `Cognito API`에 전화를 걸어 유효성을 확인한 후, 성공적인 확인을 위해 로그인 페이지로 리디렉션합니다.

```coffeescript
import * as React from 'react';
import { Redirect, RouteComponentProps } from 'react-router-dom';
import { Spin, Icon, Button, Form, notification, Input, Col } from 'antd';

// amplify
import { Auth } from 'aws-amplify';

// Presentational
import FullWidthWrapper from '../../Components/Styled/FullWidthWrapper';
import EmailConfirmFormWrapper from '../../Components/Styled/EmailConfirmFormWrapper';

type State = {
  username: string;
  loading: boolean;
  redirect: boolean;
  confirmationCode: string;
  error: string;
};

class ConfirmEmailContainer extends React.Component<RouteComponentProps, State> {
  state = {
    username: '',
    loading: false,
    redirect: false,
    confirmationCode: '',
    error: ''
  };

  componentDidMount() {
    if (this.props.location.search) {
      // get username from url params
      let username = this.props.location.search.split('=')[1];

      this.setState({ username });
    }
  }

  handleSubmit = (event: React.FormEvent) => {
    event.preventDefault();

    const { confirmationCode } = this.state;

    // show progress spinner
    this.setState({ loading: true });

    Auth.confirmSignUp(this.state.username, confirmationCode)
      .then(() => {
        this.handleOpenNotification('success', 'Succesfully confirmed!', 'You will be redirected to login in a few!');
      })
      .catch(err => {
        this.handleOpenNotification('error', 'Invalid code', err.message);
        this.setState({
          loading: false
        });
      });
  };

  handleOpenNotification = (type: string, title: string, message: string): void => {
    switch (type) {
      case 'success':
        notification'success'.trim();

    this.setState({ confirmationCode: code });

    // regex to check if string is numbers only
    const reg = new RegExp('^[0-9]+$');

    if (reg.test(code) && code.length === 6) {
      // code is a valid number

      this.setState({ loading: true });

      Auth.confirmSignUp(this.state.username, code)
        .then(() => {
          this.handleOpenNotification('success', 'Succesfully confirmed!', 'You will be redirected to login in a few!');
        })
        .catch(err => {
          this.handleOpenNotification('error', 'Invalid code', err.message);
          this.setState({
            loading: false
          });
        });
    }
  };

  handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    this.setState({ confirmationCode: event.currentTarget.value });
  };

  render() {
    const { loading, error, confirmationCode } = this.state;

    return (
      <FullWidthWrapper align="center">
        <EmailConfirmFormWrapper onSubmit={this.handleSubmit}>
          <Col md={24} lg={18}>
            <div className="full-width">
              <h2>Check your email</h2>
              <p>We've sent a six­ digit confirmation code</p>
            </div>
            <Form.Item validateStatus={error && 'error'} help={error} label="Confirmation Code">
              <Input
                size="large"
                type="number"
                placeholder="Enter confirmation code"
                onChange={this.handleChange}
                onPaste={this.handleOnPaste}
                value={confirmationCode}
              />
            </Form.Item>
          </Col>
          <Col md={24} lg={12}>
            <Button type="primary" disabled={loading} htmlType="submit" size="large">
              {loading ? <Spin indicator={<Icon type="loading" style={ fontSize: 24 } spin />} /> : 'Confirm Email'}
            </Button>
          </Col>
        </EmailConfirmFormWrapper>
         {redirect && <Redirect to={ pathname: '/login' } />}
      </FullWidthWrapper>
    );
  }
}

export default ConfirmEmailContainer;
```

그 결과 다음과 같은 결과가 나타납니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/02/password-reset-confirmation.png?resize=730%2C136&ssl=1)

### '로그인 컨테이너.tsx' 추가

사용자 코드를 확인한 후 로그인 페이지로 리디렉션합니다. 아래에서는 Ant Design에서 제공하는 우수한 검증 기능을 갖춘 양식을 작성하겠습니다.

유효성 검사가 통과된 후, 우리는 AWS SDK에 포함된 `Auth` 모듈을 사용하여 사용자 이름과 암호 페이로드를 제출하고, 그러면 `Cognito API`에 호출하여 성공 또는 오류 페이로드를 반환합니다. 사용자 자격 증명이 통과되면 토큰을 `localStorage`에 저장하고 대시보드 랜딩 페이지로 리디렉션합니다. 코드는 다음과 같습니다.

```coffeescript
import * as React from 'react';
import { Link, RouteComponentProps } from 'react-router-dom';
import { Auth } from 'aws-amplify';
import { Form, Icon, Spin, Input, Button, notification, Col, Row } from 'antd';

// Presentational 
import FormWrapper from '../../Components/Styled/FormWrapper';

// App theme 
import { colors } from '../../Themes/Colors';

// App constants
import { AUTH_USER_TOKEN_KEY } from '../../Utils/constants';

type Props = RouteComponentProps & {
  form: any;
};

type State = {
  loading: boolean;
};

class LoginContainer extends React.Component<Props, State> {
  state = {
    loading: false
  };

  handleSubmit = (event: React.FormEvent) => {
    event.preventDefault();

    this.props.form.validateFields((err: Error, values: { username: string; password: string }) => {
      if (!err) {
        let { username, password } = values;

        this.setState({ loading: true });

        Auth.signIn(username, password)
          .then(user => {
            const { history, location } = this.props;
            const { from } = location.state || {
              from: {
                pathname: '/dashboard'
              }
            };

            localStorage.setItem(AUTH_USER_TOKEN_KEY, user.signInUserSession.accessToken.jwtToken);

            notification.success({
              message: 'Succesfully logged in!',
              description: 'Logged in successfully, Redirecting you in a few!',
              placement: 'topRight',
              duration: 1.5
            });

            history.push(from);
          })
          .catch(err => {
            notification.error({
              message: 'Error',
              description: err.message,
              placement: 'topRight'
            });

            console.log(err);

            this.setState({ loading: false });
          });
      }
    });
  };

  render() {
    const { getFieldDecorator } = this.props.form;
    const { loading } = this.state;

    return (
      <React.Fragment>
        <FormWrapper onSubmit={this.handleSubmit} className="login-form">
          <Form.Item>
            {getFieldDecorator('username', {
              rules: [
                {
                  required: true,
                  message: 'Please input your username!'
                }
              ]
            })(
              <Input prefix={<Icon type="user" style={ color: colors.transparentBlack } />} placeholder="Username" />
            )}
          </Form.Item>
          <Form.Item>
            {getFieldDecorator('password', {
              rules: [
                {
                  required: true,
                  message: 'Please input your password!'
                }
              ]
            })(
              <Input
                prefix={<Icon type="lock" style={ color: colors.transparentBlack } />}
                type="password"
                placeholder="Password"
              />
            )}
          </Form.Item>
          <Form.Item className="text-center">
            <Row type="flex" gutter={16}>
              <Col lg={24}>
                <Link style={ float: 'right' } className="login-form-forgot" to="/forgot-password">
                  Forgot password
                </Link>
              </Col>
              <Col lg={24}>
                <Button
                  style={ width: '100%' }
                  type="primary"
                  disabled={loading}
                  htmlType="submit"
                  className="login-form-button"
                >
                  {loading ? <Spin indicator={<Icon type="loading" style={ fontSize: 24 } spin />} /> : 'Log in'}
                </Button>
              </Col>
              <Col lg={24}>
                Or <Link to="/signup">register now!</Link>
              </Col>
            </Row>
          </Form.Item>
        </FormWrapper>
      </React.Fragment>
    );
  }
}

export default Form.create()(LoginContainer);
```

다음과 같은 보기를 사용해야 합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/02/login-container.png?resize=403%2C418&ssl=1)

### '암호를 잊은 컨테이너.tsx' 추가

사용자가 암호를 잊어버린 경우 복구할 수 있는 방법이 필요합니다. 이것은 Amply를 사용할 때 매우 쉽다. 다음 로직을 추가하여 사용자 이름 페이로드를 취한 후 확인 코드를 사용자의 전자 메일로 전송하여 암호를 재설정하는 데 사용할 것입니다.

```coffeescript
import * as React from 'react';
import { Link, Redirect } from 'react-router-dom';
import { Auth } from 'aws-amplify';
import { Form, Icon, Spin, Input, Button, notification, Row, Col } from 'antd';

// Presentational
import FormWrapper from '../../Components/Styled/FormWrapper';

// App theme
import { colors } from '../../Themes/Colors';

type Props = {
  form: any;
};

type State = {
  username: string;
  redirect: boolean;
  loading: boolean;
};

class ForgotPasswordContainer extends React.Component<Props, State> {
  state = {
    username: '',
    redirect: false,
    loading: false
  };

  handleSubmit = (event: React.FormEvent) => {
    event.preventDefault();

    this.props.form.validateFields((err: { message: string }, values: { username: string }) => {
      if (!err) {
        let { username } = values;

        this.setState({
          loading: true,
          username
        });

        Auth.forgotPassword(username)
          .then(data => {
            notification.success({
              message: 'Redirecting you in a few!',
              description: 'Account confirmed successfully!',
              placement: 'topRight',
              duration: 1.5,
              onClose: () => {
                this.setState({ redirect: true });
              }
            });
          })
          .catch(err => {
            notification.error({
              message: 'User confirmation failed',
              description: err.message,
              placement: 'topRight',
              duration: 1.5
            });
            this.setState({ loading: false });
          });
      }
    });
  };

  render() {
    const { getFieldDecorator } = this.props.form;
    const { loading, redirect, username } = this.state;

    return (
      <React.Fragment>
        <FormWrapper onSubmit={this.handleSubmit} className="login-form">
          <Form.Item>
            {getFieldDecorator('username', {
              rules: [
                {
                  required: true,
                  message: 'Please input your username!'
                }
              ]
            })(
              <Input prefix={<Icon type="user" style={ color: colors.transparentBlack } />} placeholder="Username" />
            )}
          </Form.Item>
          <Form.Item className="text-center">
            <Row>
              <Col lg={24}>
                <Button style={ width: '100%' } type="primary" htmlType="submit" className="login-form-button">
                  {loading ? (
                    <Spin indicator={<Icon type="loading" style={ fontSize: 24 } spin />} />
                  ) : (
                    'Confirm username'
                  )}
                </Button>
              </Col>
              <Col lg={24}>
                <Link to="/login">Ooh! Wait! I've remembered!</Link>
              </Col>
            </Row>
          </Form.Item>
        </FormWrapper>
        {redirect && (
          <Redirect
            to={
              pathname: '/reset-password',
              search: ?username=${username}
            }
          />
        )}
      </React.Fragment>
    );
  }
}

export default Form.create()(ForgotPasswordContainer);
```

양식은 다음과 같이 나타납니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/02/forgot-password-container.png?resize=404%2C315&ssl=1)

### 'PasswordResetContainer.tsx'

확인 코드가 전송된 후 암호 재설정 구성 요소로 리디렉션됩니다. 사용자가 인증 코드와 새 암호를 입력하면 `Cognito API`로 전송됩니다. 확인 코드가 일치하면 새 암호가 현재 암호로 설정되며, 이제 사용자가 이 암호를 사용하여 로그인할 수 있습니다. 코드는 다음과 같습니다.

```coffeescript
import * as React from 'react';
import { Redirect, RouteComponentProps } from 'react-router-dom';
import { Auth } from 'aws-amplify';
import { Form, Input, Icon, Button, notification, Popover, Spin, Row, Col } from 'antd';

// Presentational
import FormWrapper from '../../Components/Styled/FormWrapper';

// App theme 
import { colors } from '../../Themes/Colors';

type Props = RouteComponentProps & {
  form: any;
};

type State = {
  confirmDirty: boolean;
  redirect: boolean;
  loading: boolean;
};

class PasswordResetContainer extends React.Component<Props, State> {
  state = {
    confirmDirty: false,
    redirect: false,
    loading: false
  };

  handleBlur = (event: React.FormEvent<HTMLInputElement>) => {
    const value = event.currentTarget.value;

    this.setState({ confirmDirty: this.state.confirmDirty || !!value });
  };

  compareToFirstPassword = (rule: object, value: string, callback: (message?: string) => void) => {
    const form = this.props.form;

    if (value && value !== form.getFieldValue('password')) {
      callback('Two passwords that you enter is inconsistent!');
    } else {
      callback();
    }
  };

  validateToNextPassword = (rule: object, value: string, callback: (message?: string) => void) => {
    const form = this.props.form;
    if (value && this.state.confirmDirty) {
      form.validateFields(['confirm'], { force: true });
    }
    callback();
  };

  handleSubmit = (event: React.FormEvent) => {
    event.preventDefault();

    this.props.form.validateFieldsAndScroll((err: Error, values: { password: string; code: string }) => {
      if (!err) {
        let { password, code } = values;
        let username = this.props.location.search.split('=')[1];

        Auth.forgotPasswordSubmit(username.trim(), code.trim(), password.trim())
          .then(() => {
            notification.success({
              message: 'Success!',
              description: 'Password reset successful, Redirecting you in a few!',
              placement: 'topRight',
              duration: 1.5,
              onClose: () => {
                this.setState({ redirect: true });
              }
            });
          })
          .catch(err => {
            notification'error'}
                </Button>
              </Col>
            </Row>
          </Form.Item>
        </FormWrapper>
        {redirect && <Redirect to={ pathname: '/login' } />}
      </React.Fragment>
    );
  }
}

export default Form.create()(PasswordResetContainer);
```

구성 요소에 다음과 유사한 보기가 표시됩니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2020/02/password-reset-container.png?resize=662%2C437&ssl=1)

## 결론

NAT은 개발자로서의 삶을 편리하게 해 주는 매우 안정적인 콤보인 AWS Cognito 및 Amply를 사용하여 등록, 로그인 및 암호 재설정을 다루었습니다. 앱 인증 걱정 없이 비즈니스 로직을 구현하는 데 집중할 수 있습니다.

또한 AWS Cognito 및 Amply는 모노리스 또는 마이크로 서비스 중 어떤 아키텍처 패턴과도 통합할 수 있습니다. 여기서 코드에 액세스할 수 있으며, 여기서 데모 앱을 사용해 볼 수도 있습니다.