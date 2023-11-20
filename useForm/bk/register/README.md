# register

`(name: string, RegisterOptions?) => ({ onChange, onBlur, name, ref })`<br/>
이 메서드를 사용하면 입력 또는 선택 요소를 등록하고 유효성 검사 규칙을 React Hook Form에 적용할 수 있습니다. 유효성 검사 규칙은 모두 HTML 표준을 기반으로 하며 사용자 정의 유효성 검사 방법도 허용합니다.

등록 함수를 호출하고 입력의 이름을 제공하면 다음과 같은 메서드가 반환됩니다.

| Name     | Type           | Description                                               |
| -------- | -------------- | --------------------------------------------------------- |
| onChange | ChangeHandler  | onChange 프롭을 호출하여 입력 변경 이벤트를 구독합니다.   |
| onBlur   | ChangeHandler  | onBlur 프로퍼티를 사용하여 입력 블러 이벤트를 구독합니다. |
| ref      | React.Ref<any> | Hook Form에 등록하기 위한 입력 참조입니다.                |
| name     | string         | 등록되는 입력의 이름입니다.                               |

<br/>

| input Name                   | Submit Result                      |
| ---------------------------- | ---------------------------------- |
| register("firstName")        | {firstName: 'value'}               |
| register("name.firstName")   | {name: { firstName: 'value' }}     |
| register("name.firstName.0") | {name: { firstName: [ 'value' ] }} |

#### ❗Return

```jsx
const { onChange, onBlur, name, ref } = register('firstName');

<input
  onChange={onChange}
  onBlur={onBlur}
  name={name}
  ref={ref}
/>

//위의 문법과 아래의 문법은 똑같으니, 아래로 사용한다.
<input {...register('firstName')} />
```

#### ❗Options

유효성 검사만 등록하거나, 오류 메시지와 함께 등록하는 방법이 있다.<br/>
Type 참고는 document를 봐주세요. \* https://react-hook-form.com/docs/useform/register

| Name      | Description                                                                                                                                | Code Examples(유효성 검사만)                     | Code Examples(유효성 검사와 에러메세지)                                            |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------ | ---------------------------------------------------------------------------------- |
| ref       | React element ref                                                                                                                          | `<input {...register("ex")} />`                  |
| required  | 제출하기 전에 필수로 입력된 값이 있어야 하는지를 나타내는 boolean입니다. 오류 객체에서 오류 메시지를 반환하는 문자열을 지정할 수 있습니다. | `<input {...register("ex", {required: true})}/>` | `<input {...register("ex", {required: 'error message' })}/>`                       |
| maxLength | 허용할 값의 최대 길이입니다.                                                                                                               | `<input {...register("ex", {maxLength: 2})}/>`   | `<input {...register("ex", {maxLength: : {value: 2,message: 'error message'}})}/>` |
| minLength | 허용할 값의 최소 길이입니다.                                                                                                               | `<input {...register("ex", {minLength: 2})}/>`   | `<input {...register("ex", {minLength:{value: 2,message: 'error message'}})}/>`    |
