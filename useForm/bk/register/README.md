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

| Name             | Description                                                                                                                                                                                                                            | Code Examples(유효성 검사만)                                                                                                                                                                                                                                                                                                                         | Code Examples(유효성 검사와 에러메세지)                                            |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| ref              | React element ref                                                                                                                                                                                                                      | `<input {...register("ex")} />`                                                                                                                                                                                                                                                                                                                      |
| required         | 제출하기 전에 필수로 입력된 값이 있어야 하는지를 나타내는 boolean입니다. 오류 객체에서 오류 메시지를 반환하는 문자열을 지정할 수 있습니다.                                                                                             | `<input {...register("ex", {required: true})}/>`                                                                                                                                                                                                                                                                                                     | `<input {...register("ex", {required: 'error message' })}/>`                       |
| maxLength        | 허용할 값의 최대 길이입니다.                                                                                                                                                                                                           | `<input {...register("ex", {maxLength: 2})}/>`                                                                                                                                                                                                                                                                                                       | `<input {...register("ex", {maxLength: : {value: 2,message: 'error message'}})}/>` |
| minLength        | 허용할 값의 최소 길이입니다.                                                                                                                                                                                                           | `<input {...register("ex", {minLength: 2})}/>`                                                                                                                                                                                                                                                                                                       | `<input {...register("ex", {minLength:{value: 2,message: 'error message'}})}/>`    |
| max              | 이 입력에 허용할 최대값입니다.                                                                                                                                                                                                         | `<input type="number" {...register("ex", {max: 2})}/>`                                                                                                                                                                                                                                                                                               | ``                                                                                 |
| min              | 이 입력에 허용할 최소값입니다.                                                                                                                                                                                                         | `<input type="number" {...register("ex", {min: 2})}/>`                                                                                                                                                                                                                                                                                               | ``                                                                                 |
| pattern          | 입력에 대한 정규식 패턴입니다.                                                                                                                                                                                                         | `<input {...register("ex", {pattern: /[A-Za-z]{3}/})}/>`                                                                                                                                                                                                                                                                                             | ``                                                                                 |
| validate         | 콜백 함수를 인수로 전달하여 유효성을 검사하거나 콜백 함수의 객체를 전달하여 모든 함수의 유효성을 검사할 수 있습니다. 이 함수는 필수 속성에 포함된 다른 유효성 검사 규칙에 의존하지 않고 자체적으로 실행됩니다.                         | `<input {...register("ex", {validate: (value, formValues) => value === '1'})}/>` <br/> // object of callback functions<br/> `<input {...register("ex", {validate: {positive: v => parseInt(v) > 0,lessThanTen: v => parseInt(v) < 10,validateNumber: (\_, values) => !!(values.number1 + values.number2), checkUrl: async () => await fetch()}})}/>` | ``                                                                                 |
| valueAsNumber    | 숫자를 반환합니다.(문제가 발생 시 NaN이 반환) 유효성 검사 전에 발생합니다. "type=number" 에만 적용되고 지원되지만, 데이터 조작 없이 숫자를 던집니다. defaultValue 또는 defaultValues를 변환하지 않습니다.                              | `<input type="number" {...register("ex", {valueAsNumber: true})}/>`                                                                                                                                                                                                                                                                                  | ``                                                                                 |
| valueAsDate      | Date 객체를 반환합니다.(문제가 발생 시 Invalid Date 반환) 유효성 검사 전에 발생합니다. `<input />` 에만 적용됩니다. defaultValue 또는 defaultValues를 변환하지 않습니다.                                                               | `<input type="date" {...register("ex", {valueAsDate: true})}/>`                                                                                                                                                                                                                                                                                      | ``                                                                                 |
| setValueAs       | 함수를 실행하여 입력값을 반환합니다. 유효성 검사 전에 발생합니다. 또한, valueAsNumber 또는 valueAsDate가 참이면 setValueAs는 무시됩니다. 텍스트 입력에만 적용됩니다.                                                                   | `<input type="number" {...register("ex", {setValueAs: v => parseInt(v),})}/>`                                                                                                                                                                                                                                                                        | ``                                                                                 |
| disabled         | disabled를 true로 설정하면 입력 값이 정의되지 않고 입력 제어가 비활성화됩니다.비활성화하면 기본 제공 유효성 검사 규칙도 생략됩니다.스키마 유효성 검사의 경우 입력 또는 컨텍스트 객체에서 반환된 정의되지 않은 값을 활용할 수 있습니다. | `<input {...register("ex", {disabled: true})}/>`                                                                                                                                                                                                                                                                                                     | ``                                                                                 |
| onChange         | 변경 이벤트에서 호출할 onChange 함수 이벤트를 설정합니다.                                                                                                                                                                              | `<input {...register("ex", {onChange: (e) => console.log(e)})}/>`                                                                                                                                                                                                                                                                                    | ``                                                                                 |
| onBlur           | 블러 이벤트에서 호출할 onBlur 함수 이벤트를 설정합니다.                                                                                                                                                                                | `<input {...register("ex", {onBlur: (e) => console.log(e)})}/>`                                                                                                                                                                                                                                                                                      | ``                                                                                 |
| value            | 이 프로퍼티는 useEffect 내에서 사용하거나 한 번만 호출해야 하며, 재실행할 때마다 입력한 값을 업데이트하거나 덮어씁니다.                                                                                                                | `<input {...register("ex", {value: 'bill'})}/>`                                                                                                                                                                                                                                                                                                      | ``                                                                                 |
| shouldUnregister | 마운트 해제 후 입력이 등록 해제되고 defaultValues도 제거됩니다. 참고: 이 프로퍼티는 입력 마운트 해제/재마운트 및 재정렬 후에 등록 해제 함수가 호출되므로 useFieldArray와 함께 사용할 때는 사용하지 않는 것이 좋습니다.                 | `<input {...register("ex", {shouldUnregister: true,})}/>`                                                                                                                                                                                                                                                                                            | ``                                                                                 |
| deps             | 종속 입력에 대해 유효성 검사가 트리거되며, 트리거되지 않는 등록 API로만 제한됩니다.                                                                                                                                                    | `<input {...register("ex", { deps: ['inputA', 'inputB'],})}/>`                                                                                                                                                                                                                                                                                       | ``                                                                                 |

<!-- value, shouldUnregister, deps -->

### ❗Rules

- 이름은 필수이며 고유해야 합니다(기본 라디오 및 체크박스 제외). 입력 이름은 점 및 대괄호 구문을 모두 지원하므로 중첩된 양식 필드를 쉽게 만들 수 있습니다.
- 이름은 숫자로 시작하거나 숫자를 키 이름으로 사용할 수 없습니다. 특수 문자도 사용하지 마세요.
- 점 구문은 타입스크립트 사용 일관성을 위해서만 사용하므로 배열 형식 값에는 대괄호 []가 작동하지 않습니다.<br/>

  ```jsx
  register("test.0.firstName"); // ✅
  register("test[0]firstName"); // ❌
  ```

* input을 비활성화하면 정의되지 않은 양식 값이 생성됩니다. 사용자가 입력을 업데이트하지 못하도록 하려면 readOnly을 사용하거나 전체 `<fieldset />`을 비활성화하면 됩니다. 다음은 예시입니다.

  ```tsx
  type FormValues = {
    firstName1: string;
    firstName2: string;
    lastName3: string;
  };

  let renderCount = 0;

  export default function App() {
    const { register, handleSubmit } = useForm<FormValues>();
    const onSubmit = (data: FormValues) => console.log(data);
    renderCount++;

    return (
      <div>
        <Headers
          renderCount={renderCount}
          description="Performant, flexible and extensible forms with easy-to-use validation."
        />

        <form onSubmit={handleSubmit(onSubmit)}>
          <input disabled {...register("firstName1")} placeholder="First Name1" />
          <input readOnly {...register("firstName2")} placeholder="First Name2" />
          <fieldset disabled>
            <input {...register("lastName3")} placeholder="First Name3" />
          </fieldset>
          <input type="submit" />
        </form>
      </div>
    );
  }

  //Result
  {firstName1: Object, firstName2: "", lastName3: ""}
  ```

* 필드 배열을 생성하려면 입력 이름 뒤에 점과 숫자를 붙여야 합니다. 예: test.0.data
* 렌더링할 때마다 이름을 변경하면 새 입력이 등록됩니다. 등록된 각 입력에 대해 정적 이름을 유지하는 것이 좋습니다.
* 입력 값과 참조는 더 이상 마운트 해제에 따라 제거되지 않습니다. 등록 취소를 호출하여 해당 값과 참조를 제거할 수 있습니다.
<!-- * Individual register option can't be removed by undefined or {}. You can update individual attribute instead. -->
