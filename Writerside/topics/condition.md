# Hướng dẫn sử dụng component ConditionPopup

## Import

```Typescript
<ConditionPopup
  open={visiblePopup}
  setOpen={setVisiblePopup}
  handleSubmit={(value: any, form: FormInstance) => {
    console.log('submit', value, form.getFieldsValue(true))
  }}
  initialValue={exampleCondition}
  declareObject={declareObjectExample}
/>
```

## Props

open, setOpen, handleSubmit, là các trường common trong các component popup dùng để hiển thị và ẩn popup, xử lý submit form.

## initialValue

initialValue: giá trị ban đầu của form, dùng để hiển thị giá trị ban đầu của form. Thường là giá trị đã được lưu trước đó. (Trong màn edit)

 - Cấu trúc:
```Typescript
  [
      <!-- Array: Nhóm điều kiện -->
      [
        <!-- Điều kiện bên trong là dạng Object -->
        {
          // Condition: Chọn điều kiện so sánh
          condition: { 
            type: 'select', // Hiện tại có 2 dạng chính là input và select
            options: [ // Các option của select
              { value: 'A', label: 'A' },
              { value: 'B', label: 'B' },
              { value: 'C', label: 'C' },
            ],
            value: 'A', // Giá trị trong Select (Có thể tự động generate khi chọn option)
            /**
             * onChange: Hàm xử lý khi thay đổi giá trị của Select có chút khác biệt so với antd
             * *  name: giống với tên của field trong Form.Item của antd trong dạng array: [ 0, 2, "condition", "value" ]
             * *  form: FormInstance của antd
             * *  value: giá trị của Select
             * *  option: option của Select
             */
            onChange: (name, form, value, option) => {
              <!-- Do Something -->
              /**
               * Hàm onChooseCondition là hàm support viết thêm để xử lý khi chọn điều kiện
               * Chức năng: Thay đổi giá trị condition, operator, value khi biết name, form
               * Có 4 tham số:
               * - data: Object chứa giá trị cần thay đổi (condition, operator, value)
               * - name: Tên của field trong Form.Item của antd
               * - form: FormInstance của antd
               * - option?: Loại thay đổi REPLACE hoặc MERGE (Thay đổi hoặc gộp thêm) - Mặc định là REPLACE
               */
                onChooseCondition(
                  {
                    condition: {
                      value: 'C', // Thay đổi giá trị của condition
                    },
                    value: { // Thêm mới giá trị của value
                      type: 'select',
                      placeholder: 'Giá trị',
                    },
                  },
                  name, // Example: [ 0, 2, "condition", "value" ]
                  form,
                  'MERGE', // MERGE hoặc REPLACE
                )
              },
            // Các trường dữ liệu khác tùy theo type và có props giống với antd như placeholder, disabled, ...
          },
          // Tương tự với Operator và Value, trong đó Operator là phép so sánh (Toán tử), Value là giá trị so sánh
          operator: {},
          value: {},
        }
      ]
    ] 
```

 - Data example:
  ```Typescript
  export const exampleCondition = [
  // Nhóm điều kiện
  [
    <!-- Nhóm điều kiện có 2 điều kiện con bên trong -->
    {
      id: 1, // id trong này không có ý nghĩa gì cả, chỉ để phân biệt các điều kiện với nhau
      condition: {
        type: 'select',
        options: [
          { value: 'A', label: 'A' },
          { value: 'B', label: 'B' },
          { value: 'C', label: 'C' },
        ],
        value: 'A',
        placeholder: 'Giá trị A',
      },
      operator: {
        type: 'select',
        options: [
          { value: 'A', label: 'A' },
          { value: 'B', label: 'B' },
          { value: 'C', label: 'C' },
        ],
        placeholder: 'Giá trị B',
      },
      value: {
        type: 'select',
        options: [
          { value: 'A', label: 'A' },
          { value: 'B', label: 'B' },
          { value: 'C', label: 'C' },
        ],
        placeholder: 'Giá trị C',
      },
    },
    {
      condition: {
        type: 'select',
        options: [
          { value: 'A', label: 'A' },
          { value: 'B', label: 'B' },
          { value: 'C', label: 'C' },
        ],
        placeholder: 'Giá trị A',
      },
      operator: {
        type: 'select',
        options: [
          { value: 'A', label: 'A' },
          { value: 'B', label: 'B' },
          { value: 'C', label: 'C' },
        ],
        placeholder: 'Giá trị B',
      },
      value: {
        type: 'input',
        options: [
          { value: 'A', label: 'A' },
          { value: 'B', label: 'B' },
          { value: 'C', label: 'C' },
        ],
      },
    },
  ],
  <!-- Nhóm điều kiện có 1 điều kiện con bên trong -->
  [
    {
      id: 3,
      condition: {
        type: 'select',
        options: [
          { value: 'A', label: 'A' },
          { value: 'B', label: 'B' },
          { value: 'C', label: 'C' },
        ],
        placeholder: 'Giá trị A',
      },
      operator: {
        type: 'select',
        options: [
          { value: 'A', label: 'A' },
          { value: 'B', label: 'B' },
          { value: 'C', label: 'C' },
        ],
        placeholder: 'Giá trị B',
      },
      value: {
        type: 'select',
        options: [
          { value: 'A', label: 'A' },
          { value: 'B', label: 'B' },
          { value: 'C', label: 'C' },
        ],
        placeholder: 'Giá trị C',
      },
    },
  ],
]
```

## declareObject


 - Cấu trúc: Các giá trị newData, groupData giống với cấu trúc phía trên, nhưng có thêm các hàm xử lý trước và sau khi thêm, xóa điều kiện.
 
`Lưu ý: groupData là mảng các điều kiện nên khi khai báo cần để dạng [{...}, ...]`

```Typescript
declareObject: {
    delete?: {
        beforeDelete?: (index: number, name: string | number | (string | number)[], form: FormInstance<any>) => void;
        afterDelete?: (index: number, name: string | number | (string | number)[], form: FormInstance<any>) => void;
    };
    add?: {
        beforeAdd?: (name: string | number | (string | number)[], form: FormInstance<any>) => void;
        afterAdd?: (name: string | number | (string | number)[], form: FormInstance<any>) => void;
        newData?: {
            condition: {
                type: 'select' | 'input';
                options?: { value: string; label: string }[];
                value?: string;
                onChange?: (name: string | number | (string | number)[], form: FormInstance<any>, value: string, option: { value: string; label: string }) => void;
            };
            operator: {
                ...
            };
            value: {
                ...                
            };
        };
        groupData?: {
            condition: {
                type: 'select' | 'input';
                options?: { value: string; label: string }[];
                value?: string;
                onChange?: (name: string | number | (string | number)[], form: FormInstance<any>, value: string, option: { value: string; label: string }) => void;
                {...props}: SelectProps;
            };
            operator: {
                ...
            };
            value: {
                ...                
            };
        }[];
    };
};
```

- Data example:
```Typescript
    export const declareObjectExample = {
  add: {
    beforeAdd: (name: any, form: any) => {
      console.log('beforeAdd', name, form.getFieldsValue(true))
    },
    groupData: [
      {
        condition: {
          type: 'select',
          placeholder: 'Điều kiện BER',
          options: [
            { value: 'A', label: 'A' },
            { value: 'B', label: 'B' },
            { value: 'C', label: 'C' },
          ],
        },
        operator: {
          type: 'select',
          placeholder: 'Toán tử',
          options: [
            { value: 'A', label: '=' },
            { value: 'B', label: '<' },
            { value: 'C', label: '>' },
          ],
        },
        value: {
          type: 'select',
          placeholder: 'Giá trị',
          options: [
            { value: 'A', label: 'A' },
            { value: 'B', label: 'B' },
            { value: 'C', label: 'C' },
          ],
        },
      },
    ],
    newData: {
      condition: {
        type: 'select',
        options: [
          {
            value: 'A',
            label: 'A',
          },
          {
            value: 'B',
            label: 'B',
          },
          {
            value: 'C',
            label: 'C',
          },
        ],
        onChange: (name: any, form: any, value: any, option: any) => {
          onChooseCondition(
            {
              operator: {
                type: 'select',
                placeholder: 'Toán tử !#!@#!',
                options: [
                  { value: 'A', label: '=' },
                  { value: 'B', label: '<' },
                  { value: 'C', label: '>' },
                ],
                onChange: (name: any, form: any, value: any, option: any) => {
                  onChooseCondition(
                    {
                      condition: {
                        value: 'C',
                      },
                      value: {
                        type: 'select',
                        placeholder: 'Giá trị',
                      },
                    },
                    name,
                    form,
                    'MERGE',
                  )
                },
              },
              value: {},
            },
            name,
            form,
          )
        },
        placeholder: 'ASDAS',
      },
    },
  },
}
```

## Author

[Đình Phúc](mailto: phucdd@vnpt-technology.vn)

