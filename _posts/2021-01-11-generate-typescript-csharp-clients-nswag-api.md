---
layout: post
title: "API를 기반으로 NSwag를 사용하여 TypeScript 및 C# 클라이언트 생성"
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/typescript-csharp-clients-nswag.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/typescript-csharp-clients-nswag.png?fit=730%2C487&ssl=1)

API를 위한 클라이언트를 생성하는 것은 프로젝트를 작성할 때 해야 하는 작업량을 줄일 수 있는 엄청난 방법입니다. NSwag와 같은 툴을 사용하여 신속하고 정확하게 자동 생성할 수 있는데 왜 직접 코드를 작성합니까? 문서를 인용하려면:

> NSwag 프로젝트는 Open을 생성하기 위한 도구를 제공합니다.기존 ASP의 API 사양입니다.이 Open의 NET Web API 컨트롤러 및 클라이언트 코드API 사양입니다. 이 프로젝트는 스와시버클(OpenAPI/Swagger Generation)과 오토레스트(클라이언트 생성)의 기능을 하나의 툴 체인에 결합한 것이다.

NSwag로 클라이언트를 생성하는 방법을 보여주는 몇 가지 훌륭한 게시물이 바로 a에서 직접 `nswag.json` 파일을 사용하여 말이죠.NET 프로젝트.

그러나 단순히 클라이언트 생성 기능을 위해 NSwag를 사용하려는 경우에는 어떻게 해야 합니까? 다른 언어/플랫폼으로 작성된 API를 통해 클라이언트를 생성하려는 스웨거 끝점을 표시할 수 있습니다. 어떻게 하는 거야?

또한 생성 중인 클라이언트에 대한 특별 사용자 지정을 원할 경우 `nswag.json`에서 구성하기가 어려울 수 있습니다. 이 경우 NSwag에 직접 연결하여 간단한 작업을 수행할 수 있습니다.NET 콘솔 앱.

위의 질문에 대한 답을 찾기 위해 이 게시물은 다음과 같습니다.

- 를 만듭니다.스웨거 끝점을 표시하는 NET API(또는 다른 스웨거 끝점(예: Express API)을 사용할 수 있음)
- 를 만듭니다.스웨거 끝점에서 TypeScript와 C# 클라이언트를 모두 만들 수 있는 NET 콘솔 앱
- 실행할 때 TypeScript 클라이언트를 생성하는 스크립트 생성
- 단순 TypeScript 응용프로그램에서 생성된 클라이언트를 사용하여 API 사용

계속하기 전에 Node.js와 가 모두 필요합니다.NET SDK가 설치되었습니다.

## API 생성

이제 스웨거/오픈 API 끝점을 표시하는 API를 만들겠습니다. 이 작업을 수행하는 동안 나중에 사용할 TypeScript React 앱을 만들 것입니다. 명령줄로 이동하여 를 사용하는 다음 명령을 입력합니다.NET SDK, Node 및 `create-react-app` 패키지:

```undefined
mkdir src
cd src
npx create-react-app client-app --template typescript
mkdir server-app
cd server-app
dotnet new api -o API
cd API
dotnet add package NSwag.AspNetCore
```

이제 a가 있습니다.NSwag에 종속된 NET API입니다. 생성된 Startup.cs을 다음으로 교체하여 사용하겠습니다.

```undefined
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

namespace API
{
    public class Startup
    {
        const string ALLOW_DEVELOPMENT_CORS_ORIGINS_POLICY = "AllowDevelopmentSpecificOrigins";
        const string LOCAL_DEVELOPMENT_URL = "http://localhost:3000";

        public Startup(IConfiguration configuration)
        {
            Configuration = configuration;
        }

        public IConfiguration Configuration { get; }

        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {

            services.AddControllers();

            services.AddCors(options => {
                options.AddPolicy(name: ALLOW_DEVELOPMENT_CORS_ORIGINS_POLICY,
                    builder => {
                        builder.WithOrigins(LOCAL_DEVELOPMENT_URL)
                            .AllowAnyMethod()
                            .AllowAnyHeader()
                            .AllowCredentials();
                    });
            });

            // Register the Swagger services
            services.AddSwaggerDocument();
        }

        // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure (IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            } 
            else
            {
                app.UseExceptionHandler("/Error");
                // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
                app.UseHsts ();
                app.UseHttpsRedirection();
            }

            app.UseDefaultFiles();
            app.UseStaticFiles();

            app.UseRouting();

            app.UseAuthorization();

            // Register the Swagger generator and the Swagger UI middlewares
            app.UseOpenApi();
            app.UseSwaggerUi3();

            if (env.IsDevelopment())
                app.UseCors(ALLOW_DEVELOPMENT_CORS_ORIGINS_POLICY);

            app.UseEndpoints(endpoints =>
            {
                endpoints.MapControllers();
            });
        }
    }
}
```

상기 Startup.cs에서 유의해야 할 중요한 변경 사항은 다음과 같다.

- UseOpenApi와 UseSwaggerUi3로 스웨거 끝점을 노출한다. NSwag는 애플리케이션에 모든 컨트롤러에 대해 자동으로 스웨거 끝점을 생성합니다. .NET 템플릿은 `날씨 예측 컨트롤러`와 함께 제공됩니다.
- 교차 오리진 요청(CORS) 허용 - 개발 중에 유용하며 나중에 데모를 용이하게 합니다.

프로젝트의 근본으로 돌아가서 앤피엠 프로젝트를 초기화하려고 합니다. 이를 통해 프로젝트를 보다 쉽게 진행할 수 있는 여러 편의 편리한 npm 스크립트를 배치할 예정입니다. 따라서 "npm init"을 선택하고 모든 기본값을 사용합니다.

이제 스크립트에서 사용할 종속성을 추가합니다. `npm install cpx cross-envpm-run-all-start-server-test`

우리는 또한 우리 `패키지`에 `스크립트`를 추가할 것이다.json:

```undefined
  "scripts": {
    "postinstall": "npm run install:client-app && npm run install:server-app",
    "install:client-app": "cd src/client-app && npm install",
    "install:server-app": "cd src/server-app/API && dotnet restore",
    "build": "npm run build:client-app && npm run build:server-app",
    "build:client-app": "cd src/client-app && npm run build",
    "postbuild:client-app": "cpx \"src/client-app/build/**/*.*\" \"src/server-app/API/wwwroot/\"",
    "build:server-app": "cd src/server-app/API && dotnet build --configuration release",
    "start": "run-p start:client-app start:server-app",
    "start:client-app": "cd src/client-app && npm start",
    "start:server-app": "cross-env ASPNETCORE_URLS=http://*:5000 ASPNETCORE_ENVIRONMENT=Development dotnet watch --project src/server-app/API run --no-launch-profile"
  }
```

위의 스크립트가 제공하는 내용을 살펴보겠습니다. 프로젝트의 루트에서 npm install을 실행하면 루트 패키지의 종속성만 설치되는 것이 아닙니다.postinstall, install:client-app, install:server-app 스크립트 덕분에 react 앱과 .NET 애플리케이션 종속성도 마찬가지입니다.

npm run build를 실행하면 클라이언트와 서버 앱이 구축되고 npm run start를 실행하면 React 앱과 ours가 모두 시작됩니다.넷앱. 저희 리액트 앱은 `http://localhost:3000`에서 시작됩니다. .NET 앱은 http://localhost:5000(일부 환경변수는 cross-env로 전달됨)에서 시작됩니다.

`npm run start`가 실행되면 `http://localhost:5000/swagger`에서 스웨거 끝점을 찾을 수 있습니다.

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/swagger-interface-screenshot.png?resize=730%2C440&ssl=1)

## 클라이언트 생성기 프로젝트

이제 스웨거 기반 API의 비계를 작성했으므로, 우리는 우리의 타이핑된 클라이언트를 생성할 콘솔 앱을 만들고자 합니다.

```undefined
cd src/server-app
dotnet new console -o APIClientGenerator
cd APIClientGenerator
dotnet add package NSwag.CodeGeneration.CSharp
dotnet add package NSwag.CodeGeneration.TypeScript
dotnet add package NSwag.Core
```

우리는 이제 NSwag의 코드 생성 부분에 대한 의존성을 가진 콘솔 앱을 가지고 있다. 이를 활용하기 위해 Program.cs을 변경해 보자.

```undefined
using System;
using System.IO;
using System.Threading.Tasks;
using NJsonSchema;
using NJsonSchema.CodeGeneration.TypeScript;
using NJsonSchema.Visitors;
using NSwag;
using NSwag.CodeGeneration.CSharp;
using NSwag.CodeGeneration.TypeScript;

namespace APIClientGenerator
{
    class Program
    {
        static async Task Main(string[] args)
        {
            if (args.Length != 3)
                throw new ArgumentException("Expecting 3 arguments: URL, generatePath, language");

            var url = args[0];
            var generatePath = Path.Combine(Directory.GetCurrentDirectory(), args[1]);
            var language = args[2];

            if (language != "TypeScript" && language != "CSharp")
                throw new ArgumentException("Invalid language parameter; valid values are TypeScript and CSharp");

            if (language == "TypeScript") 
                await GenerateTypeScriptClient(url, generatePath);
            else
                await GenerateCSharpClient(url, generatePath);
        }

        async static Task GenerateTypeScriptClient(string url, string generatePath) =>
            await GenerateClient(
                document: await OpenApiDocument.FromUrlAsync(url),
                generatePath: generatePath,
                generateCode: (OpenApiDocument document) =>
                {
                    var settings = new TypeScriptClientGeneratorSettings();

                    settings.TypeScriptGeneratorSettings.TypeStyle = TypeScriptTypeStyle.Interface;
                    settings.TypeScriptGeneratorSettings.TypeScriptVersion = 3.5M;
                    settings.TypeScriptGeneratorSettings.DateTimeType = TypeScriptDateTimeType.String;

                    var generator = new TypeScriptClientGenerator(document, settings);
                    var code = generator.GenerateFile();

                    return code;
                }
            );

        async static Task GenerateCSharpClient(string url, string generatePath) =>
            await GenerateClient(
                document: await OpenApiDocument.FromUrlAsync(url),
                generatePath: generatePath,
                generateCode: (OpenApiDocument document) =>
                {
                    var settings = new CSharpClientGeneratorSettings
                    {
                        UseBaseUrl = false
                    };

                    var generator = new CSharpClientGenerator(document, settings);
                    var code = generator.GenerateFile();
                    return code;
                }
            );

        private async static Task GenerateClient(OpenApiDocument document, string generatePath, Func<OpenApiDocument, string> generateCode)
        {
            Console.WriteLine($"Generating {generatePath}...");

            var code = generateCode(document);

            await System.IO.File.WriteAllTextAsync(generatePath, code);
        }
    }
}
```

우리는 우리 자신을 단순하게 만들었습니다.지정된 스웨거 URL에 대해 TypeScript 및 C# 클라이언트를 생성하는 NET 콘솔 응용 프로그램입니다. 다음 세 가지 주장을 예상합니다.

- url – 클라이언트를 생성할 swag.json 파일의 URL
- `generatePath` – 생성된 클라이언트 파일을 이 프로젝트에 상대적인 경로
- `언어` – 생성할 클라이언트의 언어. 유효한 값은 "TypeScript" 및 "CSharp"입니다.

TypeScript 클라이언트를 생성하려면 다음 명령을 사용합니다.

```undefined
dotnet run --project src/server-app/APIClientGenerator http://localhost:5000/swagger/v1/swagger.json src/client-app/src/clients.ts TypeScript
```

그러나 이 작업을 성공적으로 실행하려면 먼저 API가 실행 중인지 확인해야 합니다. 단일 명령만 있으면 다음과 같은 작업을 수행할 수 있습니다.

- API를 불러옵니다.
- 고객을 설득하다.
- API 다운

그렇게 합시다.

## "클라이언트 만들기" 스크립트 작성

프로젝트의 루트에 다음과 같은 `스크립트`를 추가할 예정입니다.

```undefined
    "generate-client:server-app": "start-server-and-test generate-client:server-app:serve http-get://localhost:5000/swagger/v1/swagger.json generate-client:server-app:generate",
    "generate-client:server-app:serve": "cross-env ASPNETCORE_URLS=http://*:5000 ASPNETCORE_ENVIRONMENT=Development dotnet run --project src/server-app/API --no-launch-profile",
    "generate-client:server-app:generate": "dotnet run --project src/server-app/APIClientGenerator http://localhost:5000/swagger/v1/swagger.json src/client-app/src/clients.ts TypeScript",
```

여기서 무슨 일이 일어나고 있는지 살펴보자. npm run generate-client:server-app을 실행하면 `start-server-and-test` 패키지를 사용하여 `generate-client:server-app:serve` 스크립트를 실행하여 서버-app을 회전시킵니다.

start-server-test는 스웨거 끝점이 요청에 응답할 때까지 기다립니다. `start-server-and-test`는 응답이 시작되면 `generate-client:server-app:generate` 스크립트를 실행하여 APIC 클라이언트 Generator 콘솔 앱을 실행하고 이를 통해 스웨거를 찾을 수 있는 URL, 생성할 파일의 경로, "TypeScript"의 언어를 제공합니다.

Blazor 앱을 작성하는 경우와 같이 C# 클라이언트를 생성하려면 다음과 같이 `generate-client:server-app:generate` 스크립트를 변경할 수 있습니다.

```undefined
   "generate-client:server-app:generate": "dotnet run --project src/server-app/ApiClientGenerator http://localhost:5000/swagger/v1/swagger.json clients.cs CSharp",
```

## 생성된 API 클라이언트 사용

npm run generate-client:server-app 명령을 실행하겠습니다. clients.ts 파일을 생성해 client-app에 잘 들어맞는다. 우리는 잠시 후에 그것을 연습할 것입니다.

먼저, 앱 생성-반응 문서의 지침을 따르고 다음 사항을 `클라이언트-앱/패키지`에 추가하여 `클라이언트-앱`에서 `서버-앱`으로 프록시를 활성화하자.json:

```undefined
  "proxy": "http://localhost:5000"
```

이제 `npm run start`로 앱을 시작해 보겠습니다. 그런 다음 `App.tsx`의 내용을 다음과 같이 바꿉니다.

```undefined
import React from "react";
import "./App.css";
import { WeatherForecast, WeatherForecastClient } from "./clients";

function App() {
  const [weather, setWeather] = React.useState<WeatherForecast[] | null>();
  React.useEffect(() => {
    async function loadWeather() {
      const weatherClient = new WeatherForecastClient(/* baseUrl */ "");
      const forecast = await weatherClient.get();
      setWeather(forecast);
    }
    loadWeather();
  }, [setWeather]);

  return (
    <div className="App">
      <header className="App-header">
        {weather ? (
          <table>
            <thead>
              <tr>
                <th>Date</th>
                <th>Summary</th>
                <th>Centigrade</th>
                <th>Fahrenheit</th>
              </tr>
            </thead>
            <tbody>
              {weather.map(({ date, summary, temperatureC, temperatureF }) => (
                <tr key={date}>
                  <td>{new Date(date).toLocaleDateString()}</td>
                  <td>{summary}</td>
                  <td>{temperatureC}</td>
                  <td>{temperatureF}</td>
                </tr>
              ))}
            </tbody>
          </table>
        ) : (
          <p>Loading weather...</p>
        )}
      </header>
    </div>
  );
}

export default App;
```

위의 `Ract.use Effect`에서 자동 생성된 `Weather Forecast Client`의 새로운 인스턴스를 생성하는 것을 볼 수 있습니다. 그런 다음 WeatherClient.get()를 호출하여 GET 요청을 서버에 전송하고 강력한 방식으로 제공합니다(get()는 WeatherForecast 배열을 반환합니다). 그러면 다음과 같이 페이지에 표시됩니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/use-generated-client.gif?resize=730%2C431&ssl=1)

보시다시피, 자동 생성된 클라이언트를 사용하여 서버에서 데이터를 로드하고 있습니다. 우리는 우리가 써야 하는 코드의 양을 줄이고 오류의 가능성을 줄이고 있습니다.