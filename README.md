# NgxValidationErrors

This library allow you to show dynamically create errors in forms using **FormControlName**.

## Messages generation

For each key in form control errors object it creates a translation key that follows the template

`${validationContext}.${fieldName}.ERRORS.${errorType}`

where:
- validationContext is the form identifier (for example _USER.REGISTRATION_ default: "GENERAL")
- fieldName is the form control name in **SCREAMING_SNAKE_CASE** 
- errorType is the error key in **SCREAMING_SNAKE_CASE** 

the keys are then translated using  [@ngx-translate](https://github.com/ngx-translate/core) enriching the message using parameters taken from the error object.
if the key is not present in the language file the message fallback to `${defaultContext}.ERRORS.${errorType}` (_USER.REGISTRATION.NAME.MINLENGTH_ => _GENERAL.ERRORS.MINLENGTH_)

## Install

`npm i @xtream/ngx-validation-errors`

## Usage

Import it using
```
import {NgxValidationErrorsModule} from '@xtream/ngx-validation-errors';

@NgModule({
  imports: [
    ...
    NgxValidationErrorsModule.forRoot()
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {
}
```

now you can use validationContext and formFieldContainer it your template

```
<form [formGroup]="heroForm" validationContext="USER.REGISTRATION">
  <div formFieldContainer>
    <label>Name</label>
    <input formControlName="name"/>
  </div>
</form>
```

According to the Validators set in the FormControl the errors appears when the input is invalid, dirty and touched.

### Configuration

The library can be configured using the `forRoot` static method 

```
import {NgxValidationErrorsModule} from '@xtream/ngx-validation-errors';

@NgModule({
  declarations: [
    AppComponent,
    CustomErrorsComponent
  ],
  imports: [
    ...
    NgxValidationErrorsModule.forRoot({
      defaultContext: 'CUSTOM_GENERAL',
      errorComponent: CustomErrorsComponent
    })
  ],
  providers: [],
  entryComponents: [CustomErrorsComponent],
  bootstrap: [AppComponent]
})
export class AppModule {
}
```
 
you can set the default validation context and the errorComponent. This last one is instantiated dynamically using 
component factory and substituted to the default one, so remember to add it to the entryComponents list.
It must accept 3 input:
```
{
  messages: string[];
  params: {[key: string]: any};
  innerValidation: boolean;
}
```
