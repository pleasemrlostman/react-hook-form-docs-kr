# useForm

## </> useForm: useFormProps

useForm은 양식을 쉽게 관리하기 위한 사용자 정의 훅(custom hook)입니다.
<br/>선택적 인수로 하나의 객체를 받습니다. 다음 예시는 모든 프로퍼티와 기본값을 보여줍니다.

### ❗ Generic props:

| Option                    | Description                                                      |
| ------------------------- | ---------------------------------------------------------------- |
| defaultValues             | form의 기본 값                                                   |
| values                    | 반응형 값으로 form 값 업데이트                                   |
| resetOptions              | 새로운 form 값을 업데이트하는 동안 form의 상태를 재설정하는 옵션 |
| criteriaMode              | 모든 유효성 오류를 표시할지 아니면 하나씩 표시할지 여부          |
| shouldFocusError          | 내장 포커스 관리를 활성화 또는 비활성화할지 여부                 |
| delayError                | 오류가 즉시 나타나지 않도록 지정된 시간만큼 지연                 |
| shouldUseNativeValidation | 브라우저 기본 form 제약 조건 API를 사용할지 여부                 |
| shouldUnregister          | 언마운트 후 입력을 등록 및 해제할지 여부                         |

### ❗ Schema validation props:

| Option   | Description                                   |
| -------- | --------------------------------------------- |
| resolver | 선호하는 스키마 유효성 검사 라이브러리와 통합 |
| context  | 스키마 유효성 검사에 공급할 컨텍스트 객체     |

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
