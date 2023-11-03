사용자가 입력 필드 중 하나를 수정한 후에 isDirty 값을 true로 설정합니다. 중요한 점은 useForm에서 모든 입력 필드의 기본값을 제공해야 하며, 이렇게 하면 hook form이 폼이 수정되었는지 비교할 때 하나의 신뢰할 수 있는 출처를 갖게 됩니다.

```typescript
const {
  formState: { isDirty, dirtyFields },
  setValue,
} = useForm({ defaultValues: { test: "" } });

// isDirty: true
setValue("test", "change");

// isDirty: false because there getValues() === defaultValues
setValue("test", "");
```

위의 코드에서 isDirty 값은 사용자가 'test' 입력 필드를 'change'로 수정한 후에 true로 설정됩니다. 그러나 'test' 필드가 기본값과 같은 ' '으로 설정되면 isDirty는 false로 설정됩니다.
