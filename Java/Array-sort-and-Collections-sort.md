# Array.sort()와 Collections.sort()

## Array.sort()

- 보편적으로 배열을 정렬할 때 사용한다.
- `java.util.Arrays`에 포함되어 있어, import를 시켜야 한다.
- 기본은 정렬 기준은 오름차순으로, 숫자 > 대문자 > 소문자 > 한글순으로 정렬된다.

<br/>
<br/>

## Collections.sort()

- 보편적으로 컬렉션을 정렬할 때 사용한다.
- 기본은 정렬 기준은 오름차순으로 ,한글 > 소문자 > 대문자 > 숫자순으로 정렬된다.
- 내림차순으로 정렬하고 싶은 경우에는 `Collections.reverse()`를 사용한다.

<br/>
<br/>

> 단순 정수나 문자의 비교가 아닌 **객체를 비교**해야 할 경우에는 라이브러리의 인터페이스를 이용해야 한다.

<br/>

- 아래와 같은 객체를 비교할 경우, **기준이 필요**하다.
    - name으로 크기를 비교할 지, score로 비교할 지
    ```java
    class Student {
        String name;
        int score;
    }
    ```

<br/>
<br/>

## Comparable 인터페이스

- 객체를 비교할 때, 어떤 필드를 기준으로 잡을 지 정할 수 있다.
- 제네릭을 사용하므로, 클래스끼리 비교할 경우 클래스를 작성하면 된다.
- Comparable 인터페이스를 상속한다면, 추상 메소드인 `compareTo()`를 구현하여야 한다.
    - 이는 이미 정의 되어 있는 클래스를 상속받아서 다시 정의를 해야하는 비효율적인 면이 있다.

<br/>
<br/>

## Comparator 인터페이스

- 주로 기본 정렬 기준과는 다른 방식으로 정렬하고 싶을 때 사용한다.
- 사용법은 `Collections.sort(list, comparator);`의 형태로 사용한다.
    - 주로 **익명 객체로 사용**된다.
- Comparator 인터페이스를 상속한다면, 추상 메소드인 `compare()`를 구현하여야 한다.

<br/>

<br/>

## 실습을 통한 사용법 비교


### Collections.sort() + Comparable 인터페이스

```java
class Student implements **Comparable<Student>** {
    int score;
    String name;
 
    Student (int score, String name) {
        this.score = score;
        this.name = name;
    }
 
    public int **compareTo**(Object obj){
        Student s2 = (Student)obj;
 
        return s2.score - this.score;
    }
 
    public String toString(){
        return score + "," + name;
    }
}
 
public class ComparableTest {
    public static void main(String[] args) {
        List<Student> list = new ArrayList<>();
        list.add(new Student(20,"lee"));
        list.add(new Student(15,"choi"));
        list.add(new Student(9,"kim"));
        list.add(new Student(12,"park"));
 
        Collections.sort(list);
 
        for (Student s : list) {
            System.out.println(s.score);
        }
    }
}
```


<br/>
<br/>

### Arrays.sort() + Comparator 인터페이스

- 정렬 기준
    - 성적이 높은 2명을 정한다.
    - 성적이 같은 학생이 있다면 학번이 빠른 순으로 한다.

```java
class Student implements Comparable<Student> {
	String name; //이름
	int id; //학번
	double score; //학점
	public Student(String name, int id, double score){
		this.name = name;
		this.id = id;
		this.score = score;
	}
	public String toString(){
		return "이름: "+name+", 학번: "+id+", 학점: "+score;
	}
	/* 기본 정렬 기준: 학번 오름차순 */
	public int compareTo(Student anotherStudent) {
		return Integer.compare(id, anotherStudent.id);
	}
}

public class Main{
	public static void main(String[] args) {
		Student student[] = new Student[5];
		//순서대로 "이름", 학번, 학점
		student[0] = new Student("Dave", 20120001, 4.2);
		student[1] = new Student("Amie", 20150001, 4.5);
		student[2] = new Student("Emma", 20110001, 3.5);
		student[3] = new Student("Brad", 20130001, 2.8);
		student[4] = new Student("Cara", 20140001, 4.2);
		
		Arrays.sort(student, new Comparator<Student>(){
			public int compare(Student s1, Student s2) {
				double s1Score = s1.score;
				double s2Score = s2.score;
				if(s1Score == s2Score){ //학점이 같으면
					return Double.compare(s1.id, s2.id); //학번 오름차순
				}
				return Double.compare(s2Score, s1Score);//내림차순
			}
		});

		for(int i=0;i<5;i++)
			System.out.println(student[i]);
	}
}
```
<br/>

<br/>

----

[참고 링크](https://m.blog.naver.com/occidere/220918234464)