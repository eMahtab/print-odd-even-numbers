# Print Odd Even Numbers


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

