# go-generics
remove interface{} type value, introduce T, get fun.

## hu ?

```go
type Equer interface {
  Eq(right Equer) bool 
}
func Eq(left <T:BasicKind|*BasicKind|Equer>, right T) bool {
  switch T.(kind){
    case BasicKind:
      return left == right
    case *BasicKind:
      return left!=nil && right!=nil && *left == *right
    case Equer:
      return left.Eq(right)
  }
  return false // ?
}
func SliceEq(left []<T:BasicKind|*BasicKind|Equer>, right []T) bool {
  if len(left)!=len(right){
    return false 
  }
  for _, l := range left {
    found := false
    for _, r := range right {
      if Eq(l,r) {
       found = true
        break
      }
    }
    if found == false {
      return false
    }
  }
  return true
}

func RetInput(input <T:Any>) T {
  return input
}

func NewSliceOf(input ...<T:Any>) []T {
  ret := make([]T,0)
  ret = append(ret, inputs...)
  return ret
}

func NewOf(input <T:StringKind|*StringKind|Concater>) Whatever<T> {
  return Whatever<T>{}
}

type Concater interface {
  Concat(right <T:Struct|*Struct>) T
}

// type concatenable struct{}
// func (c concatenable) Concat(right concatenable) concatenable {return c+right}

// xx := Whatever<concatenable>{}
// xx := Whatever<string>{Value:"hello"}
// xx := Whatever<*string>{}

type Whatever<T:StringKind|*StringKind|Concater> struct{
  Value T
}
//type Whatever<T:Any> struct{}

func (w Whatever<T>) Hi() string {
  return fmt.Sprintf(`"Hi, my name is %T"`, w.Value)
}

func (w *Whatever<T>) Concat(right T) {
  switch T.(kind){
    case StringKind:
       w.Value = w.Value +right // ?
    case *StringKind:
    x := ""
    if w.Value!=nil{
      x = w.Value
    }
    if right != nil {
      x+= *right
    }
       w.Value = &x
    case Concater:
       w.Value = w.Value.Concat(right)
  }
}


func ManyTypes(x <T:Any>, xx <U:Any>) (T,U) {
  return x,xx
}

// something like that maybe...
```
