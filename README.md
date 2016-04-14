####psfromcollectionstostreamsjava8
#####cp1
```
Arrays.sort(arr,comparator)
```
Basic lambda:
```
BinaryOperator<Integer> sum = (i.j)->i+j;
BinaryOperator<Integer> sum = (i.j)->Integer.sum(i,j);
BinaryOperator<Integer> sum = Integer::sum;
```
#####cp2
######suplier:
```
Supplier<Person> p = ()->new Person();  //Or
Supplier<Person> p = Person::new;
//If want to pass params, must use 1st way
```
######function:
unary operator: take a object, return a object of same type
```
public interface UnaryOperator<T> extends Function<T,T>{}
```
```
public interface BinaryOperator<T> extends BiFunction<T,T,Y>{}
```
######implementing and()
Predicate is not a functional interface,  
multiple non-overriding abstract methods found in interface is not allowed. using default keyword to solve

#####3
######First methods on iterable collection and list
removeIf , getOrDefault, putIfAbsent...
```
List<String> name
name.replaceAll(n->n.toUpperCase());
name.replaceAll(String::toUpperCase);
name.removeIf(p->p.getAge()>10);
```
######getOrDefault()
```
Map<K,List<Person>> map = ..
map.getOrDefault(x,emptyList())
```
replace(k,newvalue)
boolean(k,oldv,newv)  if oldv exists return true

######compute, computeIfPresent, computeIfAbsent
useful to build maps of maps.
```
Map<String,Map<String,Person>> map =
map.computeIfAbsent("a",k->new HashMap<String,Person>()).put("b",person);
```
#####Map filter reduction
######second caveat.
0 is not the identity element of the max operation
```
List<Integer> list=Arrays.asList(-2,-4);
int max=0;
BinaryOperator<Integer> op = Integer::max;
for(int i:list){
    max = op.apply(max,i);
}
```
will return 0, not -2(using optional to resolve this)
```
int avg = list.stream().map(p->p.getAge()).filter(age->age>10).average()
```
#####The stream API
######empty, generator pattern
```
Stream.empty()
Stream.of(..)
Stream.generate(()->"a");
Stream.iterate("a",s->s+"a");
ThreadLocalRandom.current().ints()
```
######How to build streams on strings
```
IntStream s = "hat".chars();
Stream<String> w = Pattern.compile("[^abc]").splitAsStream(list);
######stream.builder
```
Stream.Builder s = Stream.builder();
builder.add(1).add(2).... //return result
builder.accept(1)  //..does not return result
Stream<String> stream = builder.build()
stream.forEach(...
```
after called build, we can not use add and accept method

######match reduction
anyMatch allMatch noneMatch (ternimal operation)
######Exapmle findFirst(0 findAny()
Return Optional!
```
Optional <Person> p = people.stream().findAny(p->p.getAge()>10);
```

######general reduction
1st version
```
List<Person> people = ..
int max = people.stream().reduce(0,(p1,p2)->Integer.max(p1.getAge(),p2.getAge()));
```
2nd version
```
Optional<Integer> max = people.stream().reduce((p1,p2)->Integer.max(p1.getAge(),p2.getAge()));
```
3rd: parallel version
```
List<Integer> ages = people.stream().reduce(new ArrayList<Integer>(), (list,p)->{list.add(p.getAge());return list;},
(list1,list2)->{list1.addAll(list2); return list;{
);
```
