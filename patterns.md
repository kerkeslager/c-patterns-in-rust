Note: `referenceCount` is included as a field on a lot of structures just to give an example of where one might want to include an extra metadata field.

## Tagged Unions
### C:
```
typedef enum {
  VALUE_INTEGER,
  VALUE_STRING,
  VALUE_BOOLEAN,
} ValueType;

typedef struct {
  ValueType type;
  size_t referenceCount;
  union {
    int integer;
    char* string;
    bool boolean;
  } as;
} Value;
```

Note that in this code, `sizeof(Value)` is always the same, even though `sizeof(int)`, `sizeof(char*)`, and `sizeof(bool)` may be different.

### Unsafe Rust:
?

### Safe Rust:
?

## Structure "Inheritance"
### C:
```
typedef enum {
  OBJECT_PAIR,
  OBJECT_STRING,
} ObjectType;

typedef struct {
  ObjectType type;
  size_t referenceCount;
} Object;

typedef struct {
  Object object;
  Object* car;
  Object* cdr;
} ObjectPair;

typedef struct {
  Object object;
  size_t length;
  char* characters;
} ObjectString;

Object* Object_newPair(Object* car, Object* cdr) {
  car->referenceCount++;
  cdr->referenceCount++;

  ObjectPair* self = malloc(sizeof(ObjectPair));
  self->object.type = OBJECT_PAIR;
  self->object.referenceCount = 1;
  self->car = car;
  self->cdr = cdr;
  return (Object*)self;
}

Object* Object_newString(size_t length, char* characters) {
  ObjectString* self = malloc(sizeof(ObjectString));
  self->object.type = OBJECT_STRING;
  self->object.referenceCount = 1;
  self->length = length;
  self->characters = characters;
  return (Object*)self;
}

void Object_print(Object* self) {
  switch(self->type) {
    case OBJECT_PAIR: {
      ObjectPair* pair = (ObjectPair*) self;
      printf("(");
      Object_print(pair->car);
      printf(" ");
      Object_print(pair->cdr);
      printf(")");
    } break;

    case OBJECT_STRING: {
      ObjectString* string = (ObjectString*) self;
      printf("\"%.*s\"", string->characters, string->length);
    } break;
  }
}
```

Note that `sizeof(ObjectPair)` and `sizeof(ObjectString)` may be different, and values are passed around as `Object*` which are stored on the heap.

### Unsafe Rust:
?

### Safe Rust:
?

## Final-field array
### C:
```
typedef struct {
  size_t referenceCount;
  size_t length;
  size_t allocated;
  int items[];
} IntVector;

IntVector* IntVector_new(size_t allocate) {
  IntVector* self = malloc(sizeof(IntVector) + sizeof(int) * allocate);
  self->referenceCount = 1;
  self->length = 0;
  self->allocated = allocate;
  return self;
}
```

### Unsafe Rust:
?

### Safe Rust:
?
