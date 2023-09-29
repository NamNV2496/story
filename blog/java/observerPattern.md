# Observer pattern

Observer Pattern là một trong những Pattern thuộc nhóm hành vi (Behavior Pattern). Nó định nghĩa mối phụ thuộc một – nhiều giữa các đối tượng để khi mà một đối tượng có sự thay đổi trạng thái, tất các thành phần phụ thuộc của nó sẽ được thông báo và cập nhật một cách tự động.

Observer có thể đăng ký với hệ thống. Khi hệ thống có sự thay đổi, hệ thống sẽ thông báo cho Observer biết. Khi không cần nữa, mẫu Observer sẽ được gỡ khỏi hệ thống.
<img src="blog/java/img/observerPattern1.png" style="display: block; margin-right: auto; margin-left: auto;">

## Cài đặt Observer Pattern như thế nào?

<img src="blog/java/img/observerPattern2.png" style="display: block; margin-right: auto; margin-left: auto;">


### Các thành phần tham gia Observer Pattern:

- **Subject** : chứa danh sách các observer,  cung cấp phương thức để có thể thêm và loại bỏ observer.
- **Observer** : định nghĩa một phương thức update() cho các đối tượng sẽ được subject thông báo đến khi có sự thay đổi trạng thái.
- **ConcreteSubject** : cài đặt các phương thức của Subject, lưu trữ trạng thái danh sách các ConcreateObserver, gửi thông báo đến các observer của nó khi có sự thay đổi trạng thái.
- **ConcreteObserver** : cài đặt các phương thức của Observer, lưu trữ trạng thái của subject, thực thi việc cập nhật để giữ cho trạng thái đồng nhất với subject gửi thông báo đến.

    import java.util.Observer


```java
public class ONewsChannel1 implements Observer {
    private String news;
    
    @Override
    public void update(Observable o, Object news) {
        this.setNews((String) news);
        // do anythings after the callback is called
        int a = 2 + 5;
        int b = 2 * 3;
        log.info("reciever information on concretObserver 1");
    }
}

public class ONewsChannel2 implements Observer {
    private String news;
    
    @Override
    public void update(Observable o, Object news) {
        this.setNews((String) news);
        // do anythings after the callback is called
        log.info("reciever information on concretObserver 2");
    }
}


public class NewsAgency extends Observable {
    private String news;

    public void setNews(String news) {
        this.news = news;
        /**
         * Marks this {@code Observable} object as having been changed; the
         * {@code hasChanged} method will now return {@code true}.
         */
        setChanged();

        // If this object has changed, as indicated by the hasChanged method, then notify all of its observers and then call the clearChanged method to indicate that this object has no longer changed.
        // Each observer has its update method called with two arguments: this observable object and the arg argument.
        // Params:
        // arg – any object.
        // See Also:
        // clearChanged(), hasChanged(), Observer.update(Observable, Object)
        notifyObservers(news);
    }
}


main
{
    ONewsAgency observable = new ONewsAgency();
    ONewsChannel1 observer = new ONewsChannel1();
    ONewsChannel2 observer = new ONewsChannel2();

    // register to reciever notify
    observable.addObserver(observer);
    observable.setNews("news");
}
```



Listener

```java
public class PCLNewsAgency {
    private String news;

    private PropertyChangeSupport support;

    public PCLNewsAgency() {
        support = new PropertyChangeSupport(this);
    }

    public void addPropertyChangeListener(PropertyChangeListener pcl) {
        support.addPropertyChangeListener(pcl);
    }

    public void removePropertyChangeListener(PropertyChangeListener pcl) {
        support.removePropertyChangeListener(pcl);
    }

    public void setNews(String value) {
        support.firePropertyChange("news", this.news, value);
        this.news = value;
    }
}

public class PCLNewsChannel implements PropertyChangeListener {

    private String news;

    public void propertyChange(PropertyChangeEvent evt) {
        this.setNews((String) evt.getNewValue());
    }

    // standard getter and setter
}



PCLNewsAgency observable = new PCLNewsAgency();
PCLNewsChannel observer = new PCLNewsChannel();

observable.addPropertyChangeListener(observer);
observable.setNews("news");

assertEquals(observer.getNews(), "news");

```



