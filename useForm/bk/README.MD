# useForm

## </> useForm: useFormProps

useForm은 양식을 쉽게 관리하기 위한 사용자 정의 훅(custom hook)입니다.
<br/>선택적 인수로 하나의 객체를 받습니다. 다음 예시는 모든 프로퍼티와 기본값을 보여줍니다.

### ❗ Props

#### mode: 제출하기 전 유효성 검사 전략

`onChange | onBlur | onSubmit | onTouched | all = 'onSubmit'`<br/>
이 설정은 사용자가 폼을 제출하기 전에 유효성 검사 전략을 구성하는 데 사용됩니다.<br/>
유효성 검사는 handleSubmit 함수를 호출하여 트리거되는 onSubmit 이벤트 중에 발생합니다.

| Option    | Type   | Description                                                                                                                                                    |
| --------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| onSubmit  | string | 폼 제출 이벤트에서 유효성 검사를 트리거하며,입력은 onChange 이벤트 리스너에 연결하여 스스로 유효성을 다시 검사합니다.                                          |
| onBlur    | string | 입력 요소에서 포커스를 잃은 경우 유효성 검사를 트리거합니다.                                                                                                   |
| onChange  | string | 각 입력 요소의 change 이벤트에서 유효성 검사를 트리거합니다. 이로 인해 여러 번의 다시 렌더링이 발생하므로 성능에 상당한 영향을 미칠 수 있음에 주의해야 합니다. |
| onTouched | string | 초기에는 첫 번째 포커스를 잃은 후에 유효성 검사를 트리거하며, 그 이후에는 각 변경 이벤트에서 유효성 검사를 트리거합니다.                                       |
| all       | string | 유효성 검사를 포커스를 잃는 이벤트와 변경 이벤트 모두에서 트리거합니다.                                                                                        |

#### reValidateMode: 제출 후 입력에 오류가 있는 경우 다시 유효성 검사하는 전략

`onChange | onBlur | onSubmit = 'onChange'`<br/>
이 옵션을 사용하면 사용자가 양식을 제출한 후(onSubmit 이벤트 및 handleSubmit 함수 실행) 오류가 있는 입력의 유효성을 다시 검사할 때 유효성 검사 전략을 구성할 수 있습니다. 기본적으로 재검증은 입력 변경 이벤트 중에 발생합니다.

#### defaultValues: form의 기본 값

` FieldValues | () => Promise<FieldValues>`<br/>
defaultValues는 폼의 기본 값을 설정하는 데 사용되는 속성으로, 폼 전체에 기본 값을 채웁니다. 이 속성은 동기 및 비동기 방식으로 기본 값을 할당하는 데 사용할 수 있습니다. 기본적으로 입력 필드의 기본 값을 설정할 때는 React의 defaultValue 또는 defaultChecked를 사용할 수 있지만, 전체 폼의 기본 값 설정에는 defaultValues를 사용하는 것이 권장됩니다.

예를 들어, 다음은 defaultValues를 사용하여 폼의 기본 값을 설정하는 예제입니다:

```tsx
useForm({
  defaultValues: {
    firstName: '',
    lastName: ''
  }
})

// 기본값 비동기 설정
useForm({
  defaultValues: async () => fetch('/api-endpoint');
})
```

defaultValues 설정에 대한 규칙 및 주의사항:

- undefined를 기본 값으로 제공하는 것은 제어 컴포넌트의 기본 상태와 충돌할 수 있으므로 피해야 합니다.
- defaultValues는 캐시됩니다. 이를 재설정하려면 reset API를 사용해야 합니다.
- defaultValues는 제출 결과에 기본적으로 포함됩니다.
- defaultValues로 사용하기에는 Moment 또는 Luxon과 같은 프로토타입 메서드를 포함하는 사용자 정의 객체를 피하는 것이 좋습니다.
- 폼 데이터를 포함하는 다른 옵션도 있으며, 예를 들어 register로 type="hidden"을 사용하여 데이터를 포함하거나 제출 시 데이터를 추가로 처리할 수 있습니다.

```tsx
<input {...register("hidden")} type="hidden" />;
register("hidden", { value: "data" });

// include data onSubmit
const onSubmit = (data) => {
  const output = {
    ...data,
    others: "others",
  };
};
```

#### value: 반응형 값으로 form 값 업데이트

`FieldValues`<br/>
values 속성은 외부 상태나 서버 데이터로 폼을 업데이트해야 할 때 유용한 옵션입니다. 이 옵션을 사용하면 values 속성이 변경됨에 따라 폼의 값을 반응적으로 업데이트할 수 있습니다.

예를 들어, 다음은 values를 사용하여 폼 값을 업데이트하는 예제입니다:

```tsx
function App({ values }) {
  useForm({
    values, // values 속성이 업데이트될 때 폼도 업데이트됩니다.
  });
}

function App() {
  const values = useFetch("/api");

  useForm({
    defaultValues: {
      firstName: "",
      lastName: "",
    },
    values, // 값이 반환되면 업데이트됩니다.
  });
}
```

#### resetOptions: 새로운 form 값을 업데이트하는 동안 form의 상태를 재설정하는 옵션

`KeepStateOptions`<br/>
이 속성은 값 업데이트 동작과 관련이 있으며, values 또는 defaultValues가 업데이트될 때 내부적으로 reset API가 호출됩니다. 값이 비동기적으로 업데이트된 후의 원하는 동작을 명시하는 것이 중요합니다. 이 구성 옵션은 reset 메서드의 옵션에 대한 참조입니다.

```tsx
// 기본적으로 비동기적으로 업데이트된 값이나 defaultValues는 폼 값을 재설정합니다.
useForm({ values });
useForm({ defaultValues: async () => await fetch() });

// 동작을 구성하기 위한 옵션
// 예: 사용자 상호 작용 값을 유지하고 사용자 오류를 제거하지 않고 싶을 때
useForm({
  values,
  resetOptions: {
    keepDirtyValues: true, // 사용자가 입력한 값은 유지됩니다.
    keepErrors: true, // 입력 오류가 값 업데이트와 함께 유지됩니다.
  },
});
```

#### context: 스키마 유효성 검사에 공급할 컨텍스트 객체

`object`
이 컨텍스트 객체는 변경 가능하며 리졸버의 두 번째 인수 또는 예 유효성 검사의 컨텍스트 객체에 주입됩니다.
<br/>
<br/>
이 컨텍스트 객체를 사용하면 리졸버 함수나 Yup 검증 스키마에서 사용자 정의 로직이나 추가 데이터를 활용할 수 있습니다. 이 컨텍스트 객체를 통해 다양한 데이터나 함수를 전달하여 폼 유효성 검사나 데이터 처리를 더 유연하게 제어할 수 있습니다.

예를 들어, 컨텍스트 객체를 사용하여 특정 데이터나 함수를 전달하는 방법은 다음과 같습니다:

```tsx
import { useForm } from "react-hook-form";

function App() {
  const customContext = {
    // 사용자 정의 데이터
    someData: "Hello, World",
    // 사용자 정의 함수
    customFunction: () => {
      // 여기에서 원하는 로직을 수행
    },
  };

  const { control, handleSubmit } = useForm({
    context: customContext, // 컨텍스트 객체 전달
  });

  const onSubmit = (data) => {
    // 이 함수에서 컨텍스트 객체의 데이터나 함수 사용 가능
    console.log(data);
    console.log(customContext.someData);
    customContext.customFunction();
  };

  return <form onSubmit={handleSubmit(onSubmit)}>{/* 폼 내용 */}</form>;
}
```

#### criteriaMode: 모든 유효성 오류를 표시할지 아니면 하나씩 표시할지 여부

`firstError(default) | all`<br/>

- "firstError"로 설정하면 각 필드에서 첫 번째 에러만 감지합니다.
- "all"로 설정하면 각 필드의 모든 에러를 감지합니다.

#### shouldFocusError: 내장 포커스 관리를 활성화 또는 비활성화할지 여부

`boolean = true`<br/>
유효성 검사를 시행하고 나서, 에러가 발생한 form의 focus를 시행할지 설정하는 props입니다..
focus는 register를 등록한 필드에만 맞춰지며, 등록한 순서대로 진행됩니다.

#### delayError: 오류가 즉시 나타나지 않도록 지정된 시간만큼 지연

`number`<br/>
오류 상태가 표시되는 시간을 지정된 밀리초만큼 지연시킵니다. 사용자가 오류 입력을 수정하면 오류가 즉시 제거되고 지연이 적용되지 않습니다.

#### shouldUnregister: 브라우저 기본 form 제약 조건 API를 사용할지 여부

` boolean = false`<br/>
form이 unmount되면 값이 사라지게 만들지, 아닐지 설정하는 props입니다.
기본값은 false로 설정되어 있습니다.
만약 true로 설정하고, form이 unmount되면 입력되었던 값이 모조리 사라집니다.
이 상태에서 submit하게 되면, 값은 없는 상태로 전달되기 됩니다.

#### shouldUnregister: 언마운트 후 입력을 등록 및 해제할지 여부

`boolean = false`<br/>
브라우저의 기본 유효성 검사를 활성화시키는 props입니다.
또한, :valid, :invalid와 같은 CSS 선택자를 사용하여, styling을 할 수 있습니다.

#### resolver: 선호하는 스키마 유효성 검사 라이브러리와 통합

`Resolver`<br/>
이 기능을 사용하면 Yup, Zod, Joi, Vest, Ajv 등 모든 외부 유효성 검사 라이브러리를 사용할 수 있습니다. 원하는 유효성 검사 라이브러리를 원활하게 통합할 수 있도록 하는 것이 목표입니다. 라이브러리를 사용하지 않는 경우 언제든지 자체 로직을 작성하여 양식의 유효성을 검사할 수 있습니다.<br/>
`npm install @hookform/resolvers`
| Name | Type | Description |
| --------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| values | object | 이 개체에는 전체 양식 값이 포함되어 있습니다. |
| context | object | 사용 양식 구성에 제공할 수 있는 컨텍스트 객체입니다. 렌더링할 때마다 변경할 수 있는 변경 가능한 객체입니다.
| options |{criteriaMode: string, fields: object, names: string[]} | 사용 양식의 유효성 검사된 필드, 이름 및 기준 모드에 대한 정보를 포함하는 옵션 개체입니다.

//Rules
