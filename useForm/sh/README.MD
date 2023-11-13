# useForm

## </> useForm: useFormProps

useForm 양식을 쉽게 관리하는 사용자 정의 후크입니다. 하나의 개체를 선택적 인수로 사용합니다.

<br>
  
### ❗ Option

### mode - 제출하기 전 유효성 검증

`onChange` | `onBlur` | `onSubmit` | `onTouched` | `all` ⇒ `onSubmit`

이 옵션을 사용하면 사용자가 양식을 제출하기 전에 유효성 검사를 할 수 있습니다.

유효성 검사는 handleSubmit 함수를 호출하여 트리거 되는 onSubmit 이벤트 중에 발생 합니다.

| Name      | Type   | Description                                                                                                                                      |
| --------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| onSubmit  | string | 폼을 제출할 때 트리거로 유효성 검사가 이루어지며, 입력은 onChange 이벤트 리스너를 연결하여 자체적으로 유효성 검사를 다시 합니다.                 |
| onBlur    | string | 입력 요소에서 포커스를 잃은 경우 유효성 검사를 트리거합니다.                                                                                     |
| onChange  | string | 각 입력 요소의 change 이벤트에서 유효성 검사를 트리거합니다. 이로 인해 여러 번의 다시 렌더링이 발생하므로 성능에 상당한 영향을 미칠 수 있습니다. |
| onTouched | string | 처음으로 첫 번째 포커스를 잃은 후에 유효성 검사를 트리거하며, 그 이후에는 모든 이벤트에서 유효성 검사를 트리거합니다.                            |
| all       | string | 유효성 검사를 포커스를 잃는 이벤트와 변경 이벤트 모두에서 트리거합니다.                                                                          |

<br>
  
### **reValidateMode - 제출 후 유효성 검증**

`onChage` | `onBlur` | `onSubmit` ⇒ `onChange`

이 옵션을 사용하면 사용자가 양식을 제출한 후 오류가 있을 경우, 다시 검증 될 때 까지 유효성 검사를 구성할 수 있습니다. 기본적으로 재검증은 입력 변경 이벤트 중에 발생합니다.

<br>
  
### defaultValues - 양식의 기본값

`FieldValues` | `() ⇒ Promise<FieldValues>`

폼의 기본값을 설정합니다. 동기 및 비동기 할당을 모두 사용 가능합니다. defaultValue 또는 defaultChecked를 사용하여 기본값을 설정할 수 있지만 전체 양식에 사용하는 것을 지향합니다.

- example

```jsx
useForm({
  defaultValues: {
    firstName: '',
    lastName: ''
  }
})

// 기본값을 비동기로 설정
useForm({
  defaultValues: async () => fetch('/api-endpoint');
})
```

<br>
  
> ⚠️ 규칙 <br>
> - undefined를 기본 값으로 제공하는 것은 제어되는 구성 요소의 기본 상태와 충돌하므로 지양합니다.
> - defaultValues는 캐시됩니다. 재설정을 위해서 rest API를 사용하세요.
> - 기본적으로 defaultValues는 제출 결과에 포함됩니다.
> - defaultValues로는 Moment 또는 Luxon와 같은 프로토타입 메소드가 포함된 사용자 정의 객체를 사용하지 않는 것이 좋습니다.
> - 양식 데이터를 포함하는 다른 옵션도 있습니다.


```jsx
<input {...register("hidden")} type="hidden" />;
register("hidden", { value: "data" });

// 제출 시 데이터 포함
const onSubmit = (data) => {
  const output = {
    ...data,
    others: "others",
  };
};
```
