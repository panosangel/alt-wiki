# Development - Angular

## File structure

```bash
.
├── _tools/
│   └── i18n/
├── docker/
│   └── *.*
├── e2e/
│   └── *.*
├── src/
|   ├── app/
|   |   ├── animations/
|   |   |   └── *.animation.ts
|   |   ├── decorator/
|   |   |   └── *.animation.ts
|   |   ├── domain/
|   |   |   ├── state.model.ts
|   |   |   └── *.model.ts
|   |   ├── helper/
|   |   |   └── *.helper.ts
|   |   ├── middleware/
|   |   |   └── *.ts
|   |   ├── mocks/
|   |   |   └── *.mock.ts
|   |   ├── modules/
|   |   |   ├── core/
|   |   |   |   ├── components/
|   |   |   |   |    ├── aside/
|   |   |   |   |    ├── dialog-generic/
|   |   |   |   |    └── header/
|   |   |   |   ├── domain/
|   |   |   |   |    └── interceptor.model.ts
|   |   |   |   |    └── error.handler.ts
|   |   |   |   ├── interceptors/
|   |   |   |   |    └── *.interceptor.ts
|   |   |   |   ├── core.module.ts
|   |   |   |   └── core-routing.module.ts
|   |   |   ├── login/
|   |   |   |   ├── components/
|   |   |   |   ├── login.module.ts
|   |   |   |   └── login-routing.module.ts
|   |   |   ├── standardModule/
|   |   |   |   ├── components/
|   |   |   |   ├── standardModule.module.ts
|   |   |   |   └── standardModule-routing.module.ts
|   |   |   ├── advancedModule/
|   |   |   |   ├── .../
|   |   |   |   ├── guards/
|   |   |   |   |    └── *.guard.ts
|   |   |   |   ├── pages/
|   |   |   |   |    └── pageOne/
|   |   |   |   |        ├── components/
|   |   |   |   |        ├── pageOne.module.ts
|   |   |   |   |        └── pageOne-routing.module.ts
|   |   |   |   ├── advancedModule.module.ts
|   |   |   |   └── advancedModule-routing.module.ts
│   │   │   ├── shared/
│   │   │   │   ├── components/
│   │   │   │   ├── directives/
│   │   │   │   ├── pipes/
│   │   │   │   ├── custom-libs.module.ts
│   │   │   │   ├── material.module.ts
│   │   │   │   └── shared.module.ts
│   │   │   └── styleguide/
│   │   │       ├── styleguide-routing.module.ts
│   │   │       ├── styleguide.component.html
│   │   │       ├── styleguide.component.scss
│   │   │       ├── styleguide.component.ts
│   │   │       └── styleguide.module.ts
|   |   ├── resolvers/
|   |   |   └── *.resolvers.ts
|   |   ├── service/
|   |   |   └── *.service.ts                
|   |   └── store/
|   |       ├── modelName/ 
│   │       │   ├── modelName.actions.ts
│   │       │   ├── modelName.effects.ts
│   │       │   ├── modelName.reducer.ts
│   │       │   └── modelName.selectors.ts
│   │       └── root.module.ts
│   ├── assets/
│   │   ├── fonts/
│   │   │   └── *.*
│   │   ├── i18n/
│   │   │   ├── hash.json
│   │   │   └── translate.json
│   │   └── images/
│   │       ├── logo/
│   │       │   └── *.*
│   │       └── *.*
│   ├── environments/
│   │   ├── environment.interface.ts
│   │   ├── environment.prod.ts
│   │   └── environment.ts
│   ├── browserslist
│   ├── favicon.ico
│   ├── index.html
│   ├── karma.conf.js
│   ├── main.ts
│   ├── polyfills.ts
│   ├── styles/
│   │    └── *.*
│   ├── styles.scss
│   ├── test.ts
│   ├── tsconfig.app.json
│   ├── tsconfig.spec.json
│   └── tslint.json
├── angular.json
├── build.xml
├── config.yml
├── package-lock.json
├── package.json
├── proxy-local.conf.json
├── proxy-remote.conf.json
├── tsconfig.json
└── tslint.json
```     
