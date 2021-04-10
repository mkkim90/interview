## 자바의 스레드 
자바에서 스레드를 구현하는 방법으로는 다음 두 가지가 있습니다. 
- java.lang.Runnable 인터페이스를 구현하기
- java.lang.Thread 클래스를 상속받기 

### Runnable의 인터페이스 구현

예제 코드와 같이, Runnable 인터페이스를 상속받아서 run을 재정의하였습니다. 
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

### Thread 클래스를 상속 구현

예제 코드와 같이, Thread 클래스를 상속받아서 run을 재정의하였습니다. 
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


### synchronzied 구현

synchronized키워드를 사용할 때 공유 자원에 대한 접근을 제어할 수 있습니다. 이 키워드는 메서드에 적용할 수도 있고 특정한 코드 블록에 적용할 수도 있습니다.
이 키워드를 통해 여러 스레드가 각은 객체를 동시에 실행하는 것을 방지해줍니다.

Thread를 상속받은 MyClass
```
public class MyClass extends Thread {
    private String name;
    private MyObject myObj;

    public MyClass(MyObject obj, String n) {
        name = n;
        myObj = obj;
    }

    public void run() {
        if (name.equals("1")) {
            myObj.foo(name);
        } else {
            myObj.bar(name);
        }
    }
}
```

synchronized 키워드를 붙인 메서드 foo와 bar
```
public class MyObject {
    public synchronized void foo(String name) {
        try {
            System.out.println("Thread" + name + ".foo() : starting");
            Thread.sleep(100);
            System.out.println("Thread" + name + ".foo() : ending");
        } catch (InterruptedException exc) {
            System.out.println("Thread" + name + " : interrupted.");
        }
    }

    public synchronized void bar(String name) {
        try {
            System.out.println("Thread" + name + ".bar() : starting");
            Thread.sleep(100);
            System.out.println("Thread" + name + ".bar() : ending");
        } catch (InterruptedException exc) {
            System.out.println("Thread" + name + " : interrupted.");
        }
    }
}
```

실제 구현 테스트 코드입니다.

```
public static void main(String[] args) {
        /* 서로 다른 객체인 경우 동시에 MyObject.foo() 호출이 가능하다 */
        MyObject obj1 = new MyObject();
        MyObject obj2 = new MyObject();
        MyClass thread1 = new MyClass(obj1, "1");
        MyClass thread2 = new MyClass(obj2, "2");
        thread1.start();
        thread2.start();

        /* 같은 obj를 가리키고 있는 경우에는 하나만 foo를 호출할 수 있고 다른 하나는 기다리고 있어야한다 */
        MyClass thread3 = new MyClass(obj1, "1");
        MyClass thread4 = new MyClass(obj1, "1");
        thread3.start();
        thread4.start();

        /* 설사 하나는 foo를 호출하고 있고 다른 하나는 bar를 호출하여도 기다리고 있어야한다.*/
        MyClass thread5 = new MyClass(obj1, "1");
        MyClass thread6 = new MyClass(obj1, "4");
        thread5.start();
        thread6.start();
}
```

상기 주석에 정리해놓긴 했는데, 다시 요약하면 같은 객체의 syncronized의 메서드를 서로 다른 쓰레드가 동시에 접근할 수 없습니다. 
설사, 다른 syncronized메서드를 호출하여도 동시에 접근할 수 없습니다.
하지만 서로 다른 객체의 syncronized의 메서드를 동시에 호출하는 것은 가능합니다. 


### lock 구현

좀 더 세밀하게 동기화를 제어하고 싶을 때는 락을 사용할 수 있습니다. 자원에 대한 접근을 동기화할 수 있으며, 스레드가 해당 자원을 접근하려면 우선 그 자원에 붙어 있는 락을 획득해야합니다.
락을 쥐고 있을 수 있는 스레드는 하나뿐이며, 해당 공유 자원은 한번에 한 스레드만이 사용할 수 있습니다.

```
public class LockedATM {
    private Lock lock;
    private int balance = 100;

    public LockedATM() {
        lock = new ReentrantLock();
    }

    public int withdraw(int value) {
        lock.lock();
        int temp = balance;
        try {
            Thread.sleep(100);
            temp = temp - value;
            Thread.sleep(100);
            balance = temp;
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        lock.unlock();
        return temp;
    }

    public int deposit(int value) {
        lock.lock();
        int temp = balance;
        try {
            System.out.println("입금 시작");
            Thread.sleep(100);
            temp = temp + value;
            Thread.sleep(100);
            System.out.println("입금 완료");
            balance = temp;
            System.out.println("현재 잔고 : "+ balance);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        lock.unlock();
        return temp;
    }

    public int balance() {
        return this.balance;
    }
```


테스트 코드도 빠질 수 없지요. ATMThread를 구현하였고 입급처리 메서드를 호출하도록 하였습니다.

```
public class ATMThread extends Thread {
    private int money;
    private LockedATM atm;

    public ATMThread(LockedATM atm, int money) {
        this.atm = atm;
        this.money = money;
    }

    public void run() {
        atm.deposit(money);
    }
}

public static void main(String[] args) {
        LockedATM atm = new LockedATM();
        ATMThread thread1 = new ATMThread(atm, 1000);
        ATMThread thread2 = new ATMThread(atm, 1000);
        ATMThread thread3 = new ATMThread(atm, 1000);
        thread1.start();
        thread2.start();
        thread3.start();
    }
```
상기 출력 결과는
```
입금 시작
입금 완료
현재 잔고 : 1100
입금 시작
입금 완료
현재 잔고 : 2100
입금 시작
입금 완료
현재 잔고 : 3100
```

락을 해제해보면 어떨까요?
잔고에 대한 동기화처리가 되지 않기 때문에, 하기와 같이 불규칙한 결과를 초래합니다.
```
입금 시작
입금 시작
입금 시작
입금 완료
현재 잔고 : 1100
입금 완료
입금 완료
현재 잔고 : 1100
현재 잔고 : 1100
```

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
