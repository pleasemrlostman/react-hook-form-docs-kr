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
우선 유효성 검사만 등록하는 경우

| Name(type)     | Description       | Code Examples                   |
| -------------- | ----------------- | ------------------------------- |
| ref(React.Ref) | React element ref | <input {...register("test")} /> |
