# **Asynchronous (bất đồng bộ)**

## Không phải ngẫu nhiên đây là các câu hỏi dành cho senior khi tham gia phỏng vấn. Thực sự async là 1 vấn đề khá khó và cần kinh nghiệm để xử lý

### Đầu tiên thì đặt vấn đề: Nhiều người nhầm lẫn giữa đa luồng (multithread) và bất đồng bộ (asynchoronous). Cùng phân biệt nó nhé

## **Asynchoronous (bất đồng bộ)**
- Mô hình bất đồng bộ cho phép nhiều thứ xảy ra cùng lúc khi chương trình của bạn gọi 1 long-function cần nhiều thời gian để thực hiện. và đặt biệt `nó không block chương trình lại` và khi hoàn thành chương trình sẽ biết và lấy về giá trị kết quả nếu cần 

<img src="blog/java/img/asynchoronous1.png" style="display: block; margin-right: auto; margin-left: auto;">

<img src="blog/java/img/asynchoronous2.png" style="display: block; margin-right: auto; margin-left: auto;">

- Một chương trình bất đồng bộ là tạo các chức năng thực hiện hành động chậm và có thêm một đối số, một hàm gọi lại (callback). Hành động được bắt đầu và khi nó kết thúc, chức năng gọi lại được gọi với kết quả hoặc không. Async có thể xảy ra tại nhiều thread (thường là các thread để lắng nghe callback để thực hiện tiếp tục) => nên thường bị nhầm sang multithread

<!-- Nếu muốn dùng để trả về main thread sau khi có kết quả thì sử dụng `CompletedFuture`

<img src="blog/java/img/asynchoronous3.png" style="display: block; margin-right: auto; margin-left: auto;">

result:

<img src="blog/java/img/asynchoronous4.png" style="display: block; margin-right: auto; margin-left: auto;">

if use `@Async` or ` CompletableFuture.supplyAsync()` then it will handle in another thread

<img src="blog/java/img/asynchoronous6.png" style="display: block; margin-right: auto; margin-left: auto;">

result:

<img src="blog/java/img/asynchoronous7.png" style="display: block; margin-right: auto; margin-left: auto;"> -->

## **Multithread (đa luồng)**
- MultiThreading đề cập đến việc thực hiện đồng thời/song song của nhiều hơn một bộ tuần tự (luồng) của các câu lệnh.

<img src="blog/java/img/asynchoronous5.png" style="display: block; margin-right: auto; margin-left: auto;">

<img src="blog/java/img/asynchoronous8.png" style="display: block; margin-right: auto; margin-left: auto;">


# **Ví Dụ**

- **Asynchoronous (bất đồng bộ)**

<img src="blog/java/img/asynchoronous11.png" style="display: block; margin-right: auto; margin-left: auto;">

- **Multithread (đa luồng)**

<img src="blog/java/img/asynchoronous9.png" style="display: block; margin-right: auto; margin-left: auto;">


# **Which One To Use?**
In a nutshell, for large scale applications with a lot of I/O operations and different computations, using asynchronous multithreading programming flow, will utilize the computation resources, and take care of non-blocking functions. This is the programming model of any OS!

<img src="blog/java/img/asynchoronous12
.png" style="display: block; margin-right: auto; margin-left: auto;">

<img src="blog/java/img/asynchoronous10.png" style="display: block; margin-right: auto; margin-left: auto;">

**=>** Lập trình đa luồng là tất cả về thực hiện đồng thời các chức năng khác nhau. Lập trình bất đồng bộ là về thực thi không chặn giữa các chức năng.



# CompleteFuture

To create `CompleteFuture` 

    CompletableFuture<String> completableFuture = new CompletableFuture<>();

- Phương thức complete() : cho phép quản lý kết quả trả về. Lưu ý: nếu không xác định kết quả trả về mà thực hiện phương thức get(), chương trình sẽ block mãi mãi.

- Phương thức get() : được sử dụng để lấy kết quả trả về. Phương thức này block main thread cho tới khi có kết quả trả về.

## Chạy bất đồng bộ với runAsync() và không cần kết quả trả về

    public static CompletableFuture<Void> runAsync(Runnable runnable);

Function sẽ được tách sang 1 thread khác để thực hiện độc lập vs thread main và được thực hiện

    GET http://localhost:8080/runAsync

<img src="blog/java/img/asynchoronous3.png" style="display: block; margin-right: auto; margin-left: auto;">

## Chạy bất đồng bộ với supplyAsync() và cần nhận kết quả trả về

    public static <U> CompletableFuture<U> supplyAsync(Supplier<U> supplier);

```java
    CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
        System.out.println("It is runnig in a separate thread than the main thread.");
        doSomething();
        return "Completed";
    });
```
<img src="blog/java/img/asynchoronous4.png" style="display: block; margin-right: auto; margin-left: auto;">

## Thực hiện callback với CompleteFuture

Một số phương thức được sử dụng để callback là: thenApply(), thenAccept() và thenRun().

### thenApply()

để kết hợp với supplyAsync() để thực hiện action khi có kết quả trả về.
Action sẽ được thực hiện ở 1 thread khác

    CompletableFuture.supplyAsync(supplier).thenApply().thenApply()....;
    public <U> CompletableFuture<U> thenApply(Function<? super T,? extends U> fn);

    GET http://localhost:8080/thenApply

<img src="blog/java/img/asynchoronous6.png" style="display: block; margin-right: auto; margin-left: auto;">


### thenAccept() và thenRun()
Nếu không muốn trả lại bất cứ kết quả gì từ hàm gọi lại và chỉ muốn chạy một đoạn mã sau khi hoàn thành Future, thì chúng ta có thể sử dụng các phương thức thenAccept() và thenRun()

    public CompletableFuture<Void> thenAccept(Consumer<? super T> action);
    public CompletableFuture<Void> thenRun(Runnable action);

- CompletableFuture.thenAccept() chấp nhận đối số Consumer<T> và trả về CompletableFuture<Void>. Nó có quyền truy cập vào kết quả của CompletableFuture mà nó được đính kèm (attach).
- CompletableFuture.thenRun() chấp nhận đối số là một Runable và trả về CompletableFuture<Void>. Nó không thể truy cập kết quả của CompletableFuture mà nó được đính kèm (attach).


    GET http://localhost:8080/thenAccept

<img src="blog/java/img/asynchoronous7.png" style="display: block; margin-right: auto; margin-left: auto;">

    GET http://localhost:8080/thenRun

<img src="blog/java/img/asynchoronous14.png" style="display: block; margin-right: auto; margin-left: auto;">


## Config asynchoronous

`@EnableAsync`

create a bean with special name: `asyncExecutor`

<img src="blog/java/img/asynchoronous15.png" style="display: block; margin-right: auto; margin-left: auto;">

with data:

<img src="blog/java/img/asynchoronous16.png" style="display: block; margin-right: auto; margin-left: auto;">


Call async custom config by:

<img src="blog/java/img/asynchoronous17.png" style="display: block; margin-right: auto; margin-left: auto;">

    GET http://localhost:8080/test2 

<img src="blog/java/img/asynchoronous18.png" style="display: block; margin-right: auto; margin-left: auto;">

Use `CompletedFuture` to return main thread

<img src="blog/java/img/asynchoronous19.png" style="display: block; margin-right: auto; margin-left: auto;">

Use `@Async` will return another thread

<img src="blog/java/img/asynchoronous20.png" style="display: block; margin-right: auto; margin-left: auto;">

# Use EAAsync

<img src="blog/java/img/asynchoronous21.png" style="display: block; margin-right: auto; margin-left: auto;">

to run with async and handle the return value. We use `thenApply()`

<img src="blog/java/img/asynchoronous22.png" style="display: block; margin-right: auto; margin-left: auto;">

    GET http://localhost:8080/eaAsync

<img src="blog/java/img/asynchoronous23.png" style="display: block; margin-right: auto; margin-left: auto;">


To merge future of 2 thread

```text
              @Async
              Service A(thread1,thread2) \
MicroService /                             (Merge from Response of ServiceA and ServiceB)
             \ @Async
              Service B(thread1,thread2) /
```

```text
You need to use spring's AsyncResult class to wrap your result and then use its method .completable() to return CompletableFuture object.

When merging future object use CompletableFuture.thenCompose() and CompletableFuture.thenApply() method to merge the data like this:

CompletableFuture<Integer> result = futureData1.thenCompose(fd1Value -> 
                futureData2.thenApply(fd2Value -> 
                        merge(fd1Value, fd2Value)));
```

https://stackoverflow.com/questions/60538451/spring-boot-async-with-multithreading


Ref: https://viblo.asia/p/lap-trinh-da-luong-voi-completablefuture-trong-java-8-6J3ZgBMLKmB

