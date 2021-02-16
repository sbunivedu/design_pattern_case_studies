Dependency Injection design pattern allows us to remove the hard-coded dependencies and make our application loosely coupled, extendable and maintainable.

```java
class EmailService {
  public void sendEmail(String message, String receiver){
    //logic to send email
    System.out.println("Email sent to "+receiver+ " with Message="+message);
  }
}

class MyApplication {
  private EmailService email = new EmailService();

  public void processMessages(String msg, String rec){
    //do some msg validation, manipulation logic etc
    this.email.sendEmail(msg, rec);
  }
}

public class MyLegacyTest {
  public static void main(String[] args) {
    MyApplication app = new MyApplication();
    app.processMessages("Hi Pankaj", "pankaj@abc.com");
  }
}
```

live code https://repl.it/@lubaochuan/SoggyWelloffMainframe

## Limitations:
* `MyApplication` class is responsible to initialize the email service and then use it. This leads to hard-coded dependency. If we want to switch to some other advanced email service in the future, it will require code changes in `MyApplication` class. This makes our application hard to extend and if email service is used in multiple classes then that would be even harder.
* If we want to extend our application to provide an additional messaging feature, such as SMS or Facebook message then we would need to write another application for that. This will involve code changes in application classes and in client classes too.
* Testing the application will be very difficult since our application is directly creating the email service instance. There is no way we can mock these objects in our test classes.

```java
interface MessageService {
  void sendMessage(String msg, String rec);
}

class EmailServiceImpl implements MessageService {
  @Override
  public void sendMessage(String msg, String rec) {
    //logic to send email
    System.out.println("Email sent to "+rec+ " with Message="+msg);
  }
}

class SMSServiceImpl implements MessageService {
  @Override
  public void sendMessage(String msg, String rec) {
    //logic to send SMS
    System.out.println("SMS sent to "+rec+ " with Message="+msg);
  }
}

interface Consumer {
  void processMessages(String msg, String rec);
}

class MyDIApplication implements Consumer{
  private MessageService service;

  public MyDIApplication(MessageService svc){
    this.service=svc;
  }

  @Override
  public void processMessages(String msg, String rec){
    //do some msg validation, manipulation logic etc
    this.service.sendMessage(msg, rec);
  }
}

interface MessageServiceInjector {
  public Consumer getConsumer();
}

class EmailServiceInjector implements MessageServiceInjector {
  @Override
  public Consumer getConsumer() {
    return new MyDIApplication(new EmailServiceImpl());
  }
}

class SMSServiceInjector implements MessageServiceInjector {
  @Override
  public Consumer getConsumer() {
    return new MyDIApplication(new SMSServiceImpl());
  }
}

public class MyMessageDITest {
  public static void main(String[] args) {
    String msg = "Hi Pankaj";
    String email = "pankaj@abc.com";
    String phone = "4088888888";
    MessageServiceInjector injector = null;
    Consumer app = null;

    //Send email
    injector = new EmailServiceInjector();
    app = injector.getConsumer();
    app.processMessages(msg, email);

    //Send SMS
    injector = new SMSServiceInjector();
    app = injector.getConsumer();
    app.processMessages(msg, phone);
  }
}
```

live code https://repl.it/@lubaochuan/UnselfishPromotedAfkgaming

sources:
*
