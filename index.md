# Part 1
For the test I choose `reversed` method, which inputs an array and returns *new* array in reversed order, code for the method:
```java
  static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
    return arr;
  }
```

Out of 3 tests, only one of them didn't failed, `testReversedEmpty`, code of the tests:
!(Image)[img/tf-1.png]

## Failure inducing inputs:
```java
  @Test
  public void testReversedMultiple() {
    int[] input1 = { 1, 2, 3 };
    assertArrayEquals(new int[]{3, 2, 1 }, ArrayExamples.reversed(input1));
  }
```
Where:
- Input: `{ 1, 2, 3 }`
- Expect: `{ 3, 2, 1 }`
- Output: `{ 0, 0, 0 }`

```java
  @Test
  public void testReversedSingle() {
    int[] input1 = { 42 };
    assertArrayEquals(new int[]{ 42 }, ArrayExamples.reversed(input1));
  }
```
Where:
- Input: `{ 42 }`
- Expect: `{ 42 }`
- Output: `{ 0 }`

## No Failure inputs:
```java
  @Test
  public void testReversedEmpyt() {
    int[] input1 = { };
    assertArrayEquals(new int[]{ }, ArrayExamples.reversed(input1));
  }
```
Where:
- Input: `{ }`
- Expect: `{ }`
- Output: `{ }`

## Symptom
For all inputs, method returns a new array of same length, but with all elements being 0. 

## Bug resolve
Instead of creating new array for `newArray`, we need to clone input array into it.
#### Before:
```java
  static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
    return arr;
  }
```
#### After:
```java
  static int[] reversed(int[] arr) {
    int[] newArray = arr.clone();
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
    return arr;
  }
```
Since we use `newArray` to get elements of input array in reversed order, we expect it to have the same values as input list, but in reality we don't even assign any values to it, we just create a new empty array. 

