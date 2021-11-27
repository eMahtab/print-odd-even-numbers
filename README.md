# Print Odd Even Numbers
Print the first N natural numbers using 2 Threads, in such a way that odd numbers will be printed by Odd Thread and even numbers will be printed by Even Thread.

## Implementation : wait() and notifyAll()
```java
class Printer implements Runnable {
    int remainder;
    static int counter = 1;
    static Object lock = new Object();

    Printer(int remainder) {
      this.remainder = remainder;
    }

    public void print() {
      System.out.println(Thread.currentThread().getName()+ " : " + counter);
    }

    public void run() {
      for(int i = 1; i <= 5; i++) {
          synchronized(lock) {
            while(counter % 2 != remainder) {
              try {
                lock.wait();
              } catch(InterruptedException exception) {
                exception.printStackTrace();
              }
            }
            print();
            counter++;
            lock.notifyAll();
          }
      }
    }
}

class Main {
  public static void main(String[] args) {
    Printer oddPrinter = new Printer(1);
    Printer evenPrinter = new Printer(0);
    Thread oddThread = new Thread(oddPrinter, "Odd Thread");
    Thread evenThread = new Thread(evenPrinter, "Even Thread");
    oddThread.start();
    evenThread.start();
  }
}

```

## Output :
```
Odd Thread : 1
Even Thread : 2
Odd Thread : 3
Even Thread : 4
Odd Thread : 5
Even Thread : 6
Odd Thread : 7
Even Thread : 8
Odd Thread : 9
Even Thread : 10
```
