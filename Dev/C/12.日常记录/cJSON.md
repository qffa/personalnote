# cJSON

json解析库

## JSON

JSON支持两种数据结构

- 数组 Array
- 对象 Object

cJSON提供了大量的接口来操作这两类结构

**JSON格式**

- json顶层为一个对象或数组
- 对象或数组中包含一个或多个条目（item）
- 一个item可以是字符串、整形数、浮点数或布尔值
- item也可以是另一个数组或对象


## cJSON结构体

```c
typedef struct cJSON
{
    /* next/prev allow you to walk array/object chains.
    Alternatively, use GetArraySize/GetArrayItem/GetObjectItem */
    struct cJSON *next;
    struct cJSON *prev;
    /* An array or object item will have a child pointer pointing to
    a chain of the items in the array/object. */
    struct cJSON *child;

    /* The type of the item, as above. */
    int type;

    /* The item's string, if type==cJSON_String  and type == cJSON_Raw */
    char *valuestring;
    /* writing to valueint is DEPRECATED, use cJSON_SetNumberValue instead */
    int valueint;
    /* The item's number, if type==cJSON_Number */
    double valuedouble;

    /* The item's name string, if this item is the child of,
    or is in the list of subitems of an object. */
    char *string;
} cJSON;
```

说明

- 一个`cJSON`结构体对应一个JSON item
- 同一级的item使用前后指针串成链表
- `type`为JSON item的类型，整形、浮点、字符串、布尔值、数组、对象等等
- `child`指向下一级item的链表，只有数组和对象才会有child


## cJSON常用接口

**基本操作**

```c
CJSON_PUBLIC(cJSON *) cJSON_Parse(const char *value);
CJSON_PUBLIC(char *) cJSON_Print(const cJSON *item);
CJSON_PUBLIC(char *) cJSON_PrintUnformatted(const cJSON *item);
CJSON_PUBLIC(void) cJSON_Delete(cJSON *c);
/* For analysing failed parses. This returns a pointer to the parse error.
You'll probably need to look a few chars back to make sense of it.
Defined when cJSON_Parse() returns 0. 0 when cJSON_Parse() succeeds. */
CJSON_PUBLIC(const char *) cJSON_GetErrorPtr(void);
```

**获取item**

```c
/* Returns the number of items in an array (or object). */
CJSON_PUBLIC(int) cJSON_GetArraySize(const cJSON *array);
/* Retrieve item number "item" from array "array". Returns NULL if unsuccessful. */
CJSON_PUBLIC(cJSON *)
cJSON_GetArrayItem(const cJSON *array,
                  int index);
/* Get item "string" from object. Case insensitive. */
CJSON_PUBLIC(cJSON *)
cJSON_GetObjectItem(const cJSON * const object,
                  const char * const string);
CJSON_PUBLIC(cJSON *)
cJSON_GetObjectItemCaseSensitive(const cJSON * const object,
                  const char * const string);
```

**类型判断**

```c
/* These functions check the type of an item */
CJSON_PUBLIC(cJSON_bool) cJSON_IsInvalid(const cJSON * const item);
CJSON_PUBLIC(cJSON_bool) cJSON_IsFalse(const cJSON * const item);
CJSON_PUBLIC(cJSON_bool) cJSON_IsTrue(const cJSON * const item);
CJSON_PUBLIC(cJSON_bool) cJSON_IsBool(const cJSON * const item);
CJSON_PUBLIC(cJSON_bool) cJSON_IsNull(const cJSON * const item);
CJSON_PUBLIC(cJSON_bool) cJSON_IsNumber(const cJSON * const item);
CJSON_PUBLIC(cJSON_bool) cJSON_IsString(const cJSON * const item);
CJSON_PUBLIC(cJSON_bool) cJSON_IsArray(const cJSON * const item);
CJSON_PUBLIC(cJSON_bool) cJSON_IsObject(const cJSON * const item);
CJSON_PUBLIC(cJSON_bool) cJSON_IsRaw(const cJSON * const item);
```

**创建条目**

```c
/* These calls create a cJSON item of the appropriate type. */
CJSON_PUBLIC(cJSON *) cJSON_CreateNull(void);
CJSON_PUBLIC(cJSON *) cJSON_CreateTrue(void);
CJSON_PUBLIC(cJSON *) cJSON_CreateFalse(void);
CJSON_PUBLIC(cJSON *) cJSON_CreateBool(cJSON_bool boolean);
CJSON_PUBLIC(cJSON *) cJSON_CreateNumber(double num);
CJSON_PUBLIC(cJSON *) cJSON_CreateString(const char *string);
/* raw json */
CJSON_PUBLIC(cJSON *) cJSON_CreateRaw(const char *raw);
CJSON_PUBLIC(cJSON *) cJSON_CreateArray(void);
CJSON_PUBLIC(cJSON *) cJSON_CreateObject(void);

/* These utilities create an Array of count items. */
CJSON_PUBLIC(cJSON *) cJSON_CreateIntArray(const int *numbers, int count);
CJSON_PUBLIC(cJSON *) cJSON_CreateFloatArray(const float *numbers, int count);
CJSON_PUBLIC(cJSON *) cJSON_CreateDoubleArray(const double *numbers, int count);
CJSON_PUBLIC(cJSON *) cJSON_CreateStringArray(const char **strings, int count);
```

**添加条目**

```c
/* Append item to the specified array/object. */
CJSON_PUBLIC(void) cJSON_AddItemToArray(cJSON *array, cJSON *item);
CJSON_PUBLIC(void) cJSON_AddItemToObject(cJSON *object, const char *string, cJSON *item);
/* Use this when string is definitely const (i.e. a literal, or as good as),
 * and will definitely survive the cJSON object.
 * WARNING: When this function was used, make sure to always check that (item->type & cJSON_StringIsConst) is zero before
 * writing to `item->string` */
CJSON_PUBLIC(void) cJSON_AddItemToObjectCS(cJSON *object, const char *string, cJSON *item);
/* Append reference to item to the specified array/object.
 * Use this when you want to add an existing cJSON to a new cJSON,
 * but don't want to corrupt your existing cJSON. */
CJSON_PUBLIC(void) cJSON_AddItemReferenceToArray(cJSON *array, cJSON *item);
CJSON_PUBLIC(void) cJSON_AddItemReferenceToObject(cJSON *object, const char *string, cJSON *item);
```

**移除条目**

```c
/* Remove/Detatch items from Arrays/Objects. */
CJSON_PUBLIC(cJSON *) cJSON_DetachItemViaPointer(cJSON *parent, cJSON * const item);
CJSON_PUBLIC(cJSON *) cJSON_DetachItemFromArray(cJSON *array, int which);
CJSON_PUBLIC(void) cJSON_DeleteItemFromArray(cJSON *array, int which);
CJSON_PUBLIC(cJSON *) cJSON_DetachItemFromObject(cJSON *object, const char *string);
CJSON_PUBLIC(cJSON *) cJSON_DetachItemFromObjectCaseSensitive(cJSON *object, const char *string);
CJSON_PUBLIC(void) cJSON_DeleteItemFromObject(cJSON *object, const char *string);
CJSON_PUBLIC(void) cJSON_DeleteItemFromObjectCaseSensitive(cJSON *object, const char *string);
```

**用于简化操作的宏**

```c
/* Macros for creating things quickly. */
#define cJSON_AddNullToObject(object,name) cJSON_AddItemToObject(object, name, cJSON_CreateNull())
#define cJSON_AddTrueToObject(object,name) cJSON_AddItemToObject(object, name, cJSON_CreateTrue())
#define cJSON_AddFalseToObject(object,name) cJSON_AddItemToObject(object, name, cJSON_CreateFalse())
#define cJSON_AddBoolToObject(object,name,b) cJSON_AddItemToObject(object, name, cJSON_CreateBool(b))
#define cJSON_AddNumberToObject(object,name,n) cJSON_AddItemToObject(object, name, cJSON_CreateNumber(n))
#define cJSON_AddStringToObject(object,name,s) cJSON_AddItemToObject(object, name, cJSON_CreateString(s))
#define cJSON_AddRawToObject(object,name,s) cJSON_AddItemToObject(object, name, cJSON_CreateRaw(s))
```
