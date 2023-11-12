# useFieldArray

## **useFieldArray: UseFieldArrayProps**

`useFieldArray`는 Array 형태의 form을 효율적으로 관리하기 위해 만들어진 강력한 기능입니다.

### ❗ Props

- 다음 표는 `useWatch`에 대한 파라미터에 관한 정보를 포함하고 있습니다.

| Name             | Type        | Description                                                                                                                     |
| ---------------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------- |
| name             | string      | `fieldArray`의 이름을 정의합니다. 중요한 점은 **유동적으로 바뀌는 형태의 `name`은 감지하지 못합니다.**                          |
| control          | Object      | `useForm`에서 제공해주는 `control`은 `formContext`를 사용할 경우에 `control`에 `methods`를 넣어 사용한다.                       |
| shouldUnregister | boolean     | `fieldArray`가 `unmount`상태일 경우에, 구독 상태를 해제할 것인지에 관련된 option 값 입니다.                                     |
| keyName          | string = id | `React`에서 제공하는 `key` 와 동일한 기능을 합니다. 다만 다음 react-hook-form 버전에서는 없어질 예정이라 알 필요 없어 보입니다. |
| rules            | Object      | `Register`에서 제공하는 `rules`와 동일한 기능을 제공합니다.                                                                     |

```tsx
function FieldArray() {
  const { control, register } = useForm();
  const { fields, append, prepend, remove, swap, move, insert } = useFieldArray({
    control,
    name: "test",
  });

  return (
    {fields.map((field, index) => (
      <input
        key={field.id}
        {...register(`test.${index}.value`)}
      />
    ))}
  );
}
```

### ❗ **Return**

| Name    | Type                                                          | Description                                                                                                                                                                                 |
| ------- | ------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| fields  | object & { id: string }                                       | 해당 `object`는 미리 설정해놓은 `defaultValue`나 `key`값을 가지고 있습니다.                                                                                                                 |
| append  | (obj: object object[], focusOptions) => void                  | 해당 `fieldArray`에 추가할 input을 설정하는 `function`입니다. 자동적으로 구독 상태로 전환됩니다. , **추가한 데이터는 모두 필수값으로 전환됩니다.**                                          |
| prepend | (obj: object object[], focusOptions) => void                  | 해당 `fieldArray`에 추가할 input을 설정하는 `function`입니다. 자동적으로 구독 상태로 전환됩니다. 단, 배열의 맨 앞에 추가됩니다., **추가한 데이터는 모두 필수값으로 전환됩니다.**            |
| insert  | (index: number, value: object object[], focusOptions) => void | 해당 `fieldArray`의 특정 위치에 추가할 input을 설정하는 `function`입니다. 자동적으로 구독 상태로 전환됩니다. **추가한 데이터는 모두 필수값으로 전환됩니다.**                                |
| swap    | (from: number, to: number) => void                            | 해당 `fieldArray`의 특정 데이터들의 위치를 바꾸는 `function`입니다.                                                                                                                         |
| move    | (from: number, to: number) => void                            | 해당 `fieldArray`의 특정 데이터의 위치를 변경하는 `function`입니다.                                                                                                                         |
| update  | (index: number, obj: object) => void                          | 해당 `fieldArray`의 값을 변경하는 `function`입니다. 해당 동작은 fieldArray의 unmount 와 remount 상태를 동반하기 때문에, 해당 현상을 원하지 않는 경우에는 `setValue`로 값을 변경해야 합니다. |
| replace | (obj: object[]) => void                                       | 해당 `fieldArray`의 모든 값을 변경합니다.                                                                                                                                                   |
| remove  | (index?: number number[]): number                             | 해당 `fieldArray`의 모든 값을 제거하거나, 특정 `index`의 값을 제거합니다.                                                                                                                   |

### ❗ Rules

- `useFieldArray`는 자동적으로 유니크한 id 값을 생성합니다. 이는 `React`의 Rendering과 관련된 값으로 필수로 사용해야 합니다.

```jsx
// ✅ 기본으로 제공해주는 field.id를 사용하여 key 값을 설정해야 합니다.:
{fields.map((field, index) => <input key={field.id} ... />)}

// ❌ key 값은 index를 사용하면 안됩니다.:
{fields.map((field, index) => <input key={index} ... />)}
```

- `fieldArray`에서 제공하는 `actions`(append, remove, swap...) 와 같은 options들은 중첩하여 사용하지 않는 것을 추천합니다.

```jsx
// ❌ 이런식으로 사용하지 않아야 합니다.:
onClick={() => {
  append({ test: 'test' });
  remove(0);
}}

// ✅ 좋은 예시.:
React.useEffect(() => {
  remove(0);
}, [remove])

onClick={() => {
  append({ test: 'test' });
}}
```

- 각각의 `useFieldArray`는 자신만의 state update 로직을 가지고 있기 때문에, 중복된 이름을 가진 `useFieldArray`를 사용하지 않아도 됩니다.

- 모든 `input name`은 유니크한 값을 가지고 있어야 하기 때문에, 체크박스나 라디오 처럼 같은 이름을 가진 form을 작성하려면, `useController`나 `Controller`를 사용해주세요.

- **1 depth를 가진 Array는 지원하지 않습니다.**

- 특정 값을 배열에 추가하는 경우에는 빈 값을 추가할 수 없으며, 기본에 설정된 defaultValue에 있는 모든 값들을 가지고 있어야 합니다.

```jsx
append(); ❌
append({}); ❌
append({ firstName: 'bill', lastName: 'luo' }); ✅
```

### ❗ Typescript

- name을 구독시킬 경우, 타입 설정은 `as const`로 해야 합니다.

```tsx
const { fields } = useFieldArray({
  name: `test.${index}.keyValue` as "test.0.keyValue",
});
```

- 순환 참조는 제공하지 않습니다.

- 중첩된 fieldArray의 경우, 아래와 같은 형식으로 type을 설정해주셔야 합니다.

```tsx
<input key={field.id} {...register(`test.${index}.test` as const)} />
```

### ❗ Example

```tsx
//useFieldArray
import React from "react";
import { useForm, useFieldArray } from "react-hook-form";

function App() {
  const { register, control, handleSubmit, reset, trigger, setError } = useForm(
    {
      // defaultValues: {}; 해당 옵션값을 이용해서 초기에 랜더링 할 값을 설정 할 수 있습니다.
    }
  );
  const { fields, append, remove } = useFieldArray({
    control,
    name: "test",
  });

  return (
    <form onSubmit={handleSubmit((data) => console.log(data))}>
      <ul>
        {fields.map((item, index) => (
          <li key={item.id}>
            <input {...register(`test.${index}.firstName`)} />
            <Controller
              render={({ field }) => <input {...field} />}
              name={`test.${index}.lastName`}
              control={control}
            />
            <button type="button" onClick={() => remove(index)}>
              Delete
            </button>
          </li>
        ))}
      </ul>
      <button
        type="button"
        onClick={() => append({ firstName: "bill", lastName: "luo" })}
      >
        append
      </button>
      <input type="submit" />
    </form>
  );
}
```

```tsx
//nested Form
import * as React from "react";
import { useForm, useFieldArray, useWatch } from "react-hook-form";

export default function App() {
  const { control, handleSubmit } = useForm();
  const { fields, append, update } = useFieldArray({
    control,
    name: "array",
  });

  return (
    <form onSubmit={handleSubmit((data) => console.log(data))}>
      {fields.map((field, index) => (
        <Edit
          key={field.id}
          control={control}
          update={update}
          index={index}
          value={field}
        />
      ))}

      <button
        type="button"
        onClick={() => {
          append({ firstName: "" });
        }}
      >
        append
      </button>
      <input type="submit" />
    </form>
  );
}

const Display = ({ control, index }) => {
  const data = useWatch({
    control,
    name: `array.${index}`,
  });
  return <p>{data?.firstName}</p>;
};

const Edit = ({ update, index, value, control }) => {
  const { register, handleSubmit } = useForm({
    defaultValues: value,
  });

  return (
    <div>
      <Display control={control} index={index} />

      <input
        placeholder="first name"
        {...register(`firstName`, { required: true })}
      />

      <button
        type="button"
        onClick={handleSubmit((data) => update(index, data))}
      >
        Submit
      </button>
    </div>
  );
};
```

### 🙋‍♂️ 첨언

> `useFieldArray`는 array 형식의 input을 관리하기 위해 사용하는 form이며, formContext와 함께 사용시 formContext에서 상위 root level 에서 가져온 control을 적용해 준다면, 이전에 적용해 둔 defaultValue를 자동으로 가져와 사용할 수 있다는 장점이 있다.
> 다만, `setValue`로 해당 array 값을 추가할 수 없으며, 특정 기능을 사용해서 array를 update 해주어야 한다.
