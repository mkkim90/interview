## 자바의 스레드 
자바에서 스레드를 구현하는 방법으로는 다음 두 가지가 있습니다. 
- java.lang.Runnable 인터페이스를 구현하기
- java.lang.Thread 클래스를 상속받기 

### Runnable의 인터페이스 구현의 경우, 코드 예제는 다음과 같습니다.
```
public class RunnableThreadExample implements Runnable {
    public int count = 0;

    @Override
    public void run() {
        System.out.println("RunnableThread starting");
        try {
            while (count < 5) {
                Thread.sleep(500);
                count++;
            }
        } catch (InterruptedException exc) {
            System.out.println("RunnableThread interrupted");
        }

        System.out.println("RunnableThread terminating");
    }
}
```
그리고 쓰레드에 RunnableThreadExample 객체 인스턴스를 넣어서 실행합니다.
```
public static void main(String[] args) {
        RunnableThreadExample instance = new RunnableThreadExample();
        Thread thread = new Thread(instance);
        thread.start();

        while (instance.count != 5) {
            try {
                Thread.sleep(250);
            } catch (InterruptedException exc) {
                exc.printStackTrace();
            }
        }
    }
```

### Thread 클래스를 상속 구현의 경우, 코드 예제는 다음과 같습니다.
```
public class ThreadExample extends Thread {
    int count = 0;

    public void run() {
        System.out.println("Thread starting");
        try {
            while (count < 5) {
                Thread.sleep(500);
                System.out.println("In Thread, count is " + count);
                count++;
            }
        } catch (InterruptedException exc) {
            System.out.println("Thread interrupted");
        }
        System.out.println("Thread terminating");
    }
}
```
그리고 쓰레드를 상속받았기에 ThreadExample로 직접 실행합니다.
```
public static void main(String[] args) {
        ThreadExample instance = new ThreadExample();
        instance.start();

        while (instance.count != 5) {
            try {
                Thread.sleep(250);
            } catch (InterruptedException exc) {
                exc.printStackTrace();
            }
        }
}
```


## 동기화와 락

자바는 공유 자원에 대한 접근을 제어하기 위한 동기화 방법을 제공합니다.
synchronized와 lock입니다. 


## 교착상태와 교착상태 방지
```
교착상태란, 첫번째 스레드는 두번째 스레드가 들고 있는 객체의 락이 풀리기를 기다리고 있고, 두번째 스레드 역시 첫번째 스레드가 들고 있는 
객체의 락이 풀리기를 기다리는 상황을 일컫습니다.(여러 스레드가 관계되어 있더라도 같은 상황 발생) 모든 스레드가 락이 풀리기를 기다리고 있기 때문에 무한 대기 상태에 빠지게 됩니다. 
일펀 스레드를 교착상태에 빠졌다고 합니다. 
```

### 교착 상태 충족 조건
교착 상태는 네 가지 조건이 모두 충족되어야합니다.

- 상호 배제(Mutual exclusion) :  한번에 한 프로세스만 공유 자원을 사용할 수 있습니다.  한 자원에 대한 여러 프로세스의 동시 접근 불가합니다. 
- 점유와 대기(Hold and Wait) : 공유 자원에 대한 접근 권한을 갖고 있는 프로세스가, 그 접근 권한을 양보하지 않은 상태에서 다른 자원에 대한 접근 권한을 요구 할 수 있습니다. 자원을 가지고 있는 상태에서 다른 프로세스가 사용하고 있는 자원의 반납을 기다리는 것입니다.
- 비선점(Non Preemptive) : 한 프로세스가 다른 프로세스의 자원 접근 권한을 강제 취소할 수 없습니다. 다른 프로세스의 자원을 강제로 가져올 수 없습니다.
- 환형대기(Circle wait) : 두 개 이상의 프로세스가 자원 접근을 기다리는데 그 관계에 사이클이 존재합니다. 각 프로세스가 순환적으로 다음 프로세스가 요구하는 자원을 가지고 있는 것입니다.  

### 교착상태 방지

상기 조건 가운데 하나를 제거하면 됩니다. 하지만 상기 조건 가운데 상당수는 만족되기 어렵습니다.  
많은 경우가 상호 배제 조건을 충족합니다. 한번에 한 프로세스만 사용하기 때문입니다.  그러므로 교착 방지 알고리즘(자원할당그래프 알고리즘, 은행원 알고리즘 등)은 환형 대기를 막는데 초점이 맞춰져 있습니다. 
