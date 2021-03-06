# Volatility - Part I

Here's the new code snippet with a single keyword addition, `volatile`,
to fix the infinite-loop issue:

```java runnable
class Task implements Runnable {
  volatile static boolean isDone = false;
  
  public void run() {
    try {
      System.out.println("Doing work...");
      Thread.sleep(10); // do some work
      isDone = true;
    } catch (InterruptedException ex) {
      System.out.println(ex);
    }
  }
}

class Poller implements Runnable {
  public void run() {
    int iterations = 0;
    System.out.println("Waiting for work to be done...");
    while (!Task.isDone) { // bad way to wait for a signal
      ++iterations;
    }
    System.out.println("Polled " + iterations + " times");
  }
}

public class Main {
  public static void main(String args[]) throws Exception {
    new Thread(new Poller()).start();
    new Thread(new Task()).start();
    Thread.sleep(1000); // ad-hoc wait till threads finish
  }
}
```
