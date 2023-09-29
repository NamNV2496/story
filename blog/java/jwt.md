# Authentication JWT
## 1. JWT

```text
JWT (JSON Web Tokens) là một chuẩn mở (RFC 7519) định nghĩa một cách nhỏ gọn và khép kín để truyền một cách an toàn thông tin giữa các bên dưới dạng đối tượng JSON. Thông tin này có thể được xác minh và đáng tin cậy vì nó có chứa chữ ký số.
```

- Hiểu đơn giản, dữ liệu ban đầu ở dạng JSON. Sau đó được mã hóa bằng một thuật toán nào đó nó trở thành một chuỗi string (hay chúng ta thường gọi là token) mà bằng mắt thường không thể đọc được.
- JWT có thể được mã hóa (sign) bằng một thuật toán bí mật (HMAC, SHA256) hoặc một public/private key (RSA) . JWT rất hữu ích trong việc xác thực API và ủy quyền từ server đến client.


## 2. Cấu trúc JWT
Một JWT gồm 3 phần, được ngăn cách nhau bởi dấu chấm:

- header
- payload
- signature

<img src="blog/java/img/jwt1.png" style="display: block; margin-right: auto; margin-left: auto;">
<img src="blog/java/img/jwt3.png" style="display: block; margin-right: auto; margin-left: auto;">

### 2.1 Header

Phần Header của JWT bao gồm

- typ: type - loại token
- alg: algorithm - thuật toán được sử dụng để mã hóa và giải mã. Các thuật toán này có thể là HMAC, SHA256, RSA, HS256 hoặc RS256.

```json
{
"typ": "JWT",
"alg": "HS256"
}
```

### 2.2 Payload

Phần Payload của JWT bao gồm các nội dung của thông tin và được gọi là các claim. Dưới đây là một số claim tiêu chuẩn mà chúng ta có thể sử dụng:

- iss (issuer): tổ chức phát hành token (không bắt buộc)
- sub (subject): chủ đề của token (không bắt buộc)
- aud (audience): đối tượng sử dụng token (không bắt buộc)
- exp (expired time): thời điểm token sẽ hết hạn (không bắt buộc)
- nbf (not before time): token sẽ chưa hợp lệ trước thời điểm này
- iat (issued at): thời điểm token được phát hành, tính theo UNIX time
- jti: JWT ID
Có thể có các field custom
- roles: role của token
- userId: user id

### 2.3 Signature

Signature (phần chữ ký) là phần quan trọng nhất của JWT.

Chữ ký được tính bằng cách mã hóa phần Header và Payload sử dụng Base64url Encoding và ghép chúng với một dấu chấm. Sau đó, chúng được đưa cho thuật toán mã hóa (HMAC, SHA256, RSA, HS256 hoặc RS256).
```java
// VD thuật toán chữ ký
data = base64urlEncode( header ) + "." + base64urlEncode( payload )
signature = HMAC-SHA256( data, secret_salt )
```
Do Signature đã bao gồm cả header và payload nên Signature có thể dùng để kiểm tra tính toàn vẹn của dữ liệu khi truyền tải.

## 3. Ưu điểm và Nhược điểm của JWT

JWT là một công cụ hữu ích để xác thực người dùng và cung cấp quyền truy cập. Tuy nhiên, như mọi công nghệ khác, nó cũng có những ưu điểm và nhược điểm riêng. Dưới đây là một số ưu điểm và nhược điểm của JWT:

**Ưu điểm:**

- Đơn giản và dễ sử dụng: JWT có cấu trúc đơn giản, dễ dàng để hiểu và sử dụng.
- Tiết kiệm tài nguyên: JWT được lưu trữ trên máy khách, giúp giảm tải cho máy chủ. Do đó, nó rất hữu ích trong các ứng dụng phân tán, nơi mà trạng thái có thể được lưu trữ tại nhiều vị trí khác nhau.
- Độ tin cậy cao: JWT được mã hóa và ký, giúp ngăn chặn việc sửa đổi dữ liệu trên đường truyền.
- Dữ liệu được lưu trữ trong token: JWT cho phép lưu trữ thông tin người dùng trong token, giúp giảm thiểu số lần truy vấn cơ sở dữ liệu.
- Tích hợp dễ dàng: JWT có thể tích hợp với nhiều ngôn ngữ lập trình và framework.

**Nhược điểm:**

- Không thể hủy bỏ token: Khi một token được tạo ra và gửi đến máy khách, không thể hủy bỏ nó trước khi hết thời gian sống hoặc thay đổi secret key và không thể chủ động force logout được. => có thể khắc phục bằng phương pháp mã hóa bất đối xứng sẽ được trình bày trong phần 6.
- Lưu trữ thông tin quá nhiều: Nếu lưu trữ quá nhiều thông tin người dùng trong token, kích thước của token có thể trở nên quá lớn. => khắc phục bằng cách giảm thiểu dữ liệu, chỉ lưu những thông tin cần thiết.
- Rủi ro bảo mật: Nếu secret key của JWT bị lộ, thì hacker có thể giả mạo token và truy cập thông tin người dùng. => có thể khắc phục bằng phương pháp mã hóa bất đối xứng sẽ được trình bày trong phần 6.
- Không hỗ trợ quản lý phiên: JWT không hỗ trợ quản lý phiên như các giải pháp dựa trên cookie. Điều này có nghĩa là nếu bạn muốn hủy bỏ phiên của người dùng, bạn phải chờ cho JWT hết hạn hoặc thay đổi khóa bí mật. => có thể khắc phục bằng phương pháp mã hóa bất đối xứng sẽ được trình bày trong phần 6.
- Thời gian sống của token không linh hoạt: Nếu thời gian sống của token quá ngắn, người dùng sẽ phải đăng nhập quá thường xuyên. Nếu quá dài, thì việc bảo mật sẽ bị giảm sút. => áp dụng refresh token để khắc phục


## 4. Cơ chế hoạt động JWT

<img src="blog/java/img/jwt2.png" style="display: block; margin-right: auto; margin-left: auto;">
<img src="blog/java/img/jwt4.png" style="display: block; margin-right: auto; margin-left: auto;">


- 1. Client gửi username, password tới server để login nhằm mục đích xác thực.
- 2. Nếu login thành công server sẽ tạo ra token (jwt) và gửi về client.
- 3. Client nhận token đó, rồi lưu trữ trên trình duyệt (cookies, localStorage, ..).
- 4. Khi client gửi request tiếp theo tới server, request đó sẽ được đính kèm token nhằm mục đích xác thực.
- 5. Server nhận được request và kiểm tra token. Nếu token hợp lệ thì gửi trả kết quả về client, còn không gửi thông báo chưa xác thực (403).


## **Vậy bây giờ lưu access token ở đâu?**
Chúng ta có thể bỏ qua luôn session storage và cả application state, vì chúng quá mỏng manh yếu đuối, đóng tab hoặc ctrl + F5 cái là đi mất rồi.

### **Local Storage**
Đây chính là 1 nơi nguy hiểm, bởi vì local storage có thể được truy cập từ bất kỳ script nào chạy trên cùng 1 domain (và session storage cũng thế nha).

Chúng ta gọi đó là lỗ hổng XSS, tức Cross-Site Scripting, kẻ xấu chỉ cần inject script vào target là đã có thể lấy được token.

Điểm chí mạng thứ 2 đó chính là local storage không có thời gian expire, nó sẽ còn sống mãi. Bình thường con người sống lâu thì mừng, còn token sống lâu thì lại là vấn đề 🃏

### **Cookie**
Một cách khác chính là lưu vào cookie, mặc dù cookie vẫn có thể truy cập từ script trong cùng domain bằng cách gọi document.cookie .

Nhưng nó vẫn an toàn hơn localstorage, tại sao?

Thứ nhất, cookie có thời gian expiration, không sống thọ như local storage.

Thứ 2, cookie có 1 flag gọi là httpOnly. Flag này sẽ giúp ngăn chặn cookie truy cập bởi client side.


# Tóm lại

<img src="blog/java/img/jwt6.png" style="display: block; margin-right: auto; margin-left: auto;">

## 5. Setup

<img src="blog/java/img/jwt5.png" style="display: block; margin-right: auto; margin-left: auto;">

```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.security</groupId>
        <artifactId>spring-security-config</artifactId>
        <version>5.6.0</version>
    </dependency>
    <dependency>
        <groupId>io.jsonwebtoken</groupId>
        <artifactId>jjwt</artifactId>
        <version>0.9.1</version>
    </dependency>
    <dependency>
        <groupId>javax.xml.bind</groupId>
        <artifactId>jaxb-api</artifactId>
        <version>2.3.0</version>
    </dependency>
```

### Step 1: implement UserDetailsService to get user by username

```java
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

@Configuration
@Service
public class CustomUserDetailsService implements UserDetailsService {

    @Autowired
    private IAccountRepository iAccountRepository;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // load from DB
        Optional<Account> account = iAccountRepository.findByName(username);
        if (account.isPresent()) {
            // return user create by Username and encoder password 
            // BCryptPasswordEncoder().encode(rawPassword)
            // User(String username, String password, Collection<? extends GrantedAuthority> authorities)
            return new User(account.get().getUsername(), passwordEncoder().encode(account.get().getPassword()), new ArrayList<>());
        }
        System.out.println("The user is not existence");
        return null;
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        // encode password
        return new BCryptPasswordEncoder();
    }
}
```
### Step 2: create JWTTokenUtil to generate and parsing token

```java 
    private String generateToken(String userName) {

        return Jwts.builder()
                .setSubject(userName)
                .setIssuedAt(new Date(System.currentTimeMillis()))
                .setExpiration(new Date(System.currentTimeMillis() + JWT_TOKEN_VALIDITY * 1000))
                .signWith(SignatureAlgorithm.HS512, "secret")
                .compact();
    }

     public String getUsernameFromToken(String token) {
        return getClaimFromToken(token, Claims::getSubject);
    }
    
    public <T> T getClaimFromToken(String token, Function<Claims, T> claimsResolver) {
        final Claims claims = getAllClaimsFromToken(token);
        return claimsResolver.apply(claims);
    }

    private Claims getAllClaimsFromToken(String token) {
        return Jwts.parser().setSigningKey(secret).parseClaimsJws(token).getBody();
    }

// check token expired
    public Boolean isTokenExpired(String token) {
        final Date expiration = getExpirationDateFromToken(token);
        return expiration.before(new Date());
    }

// can use another way Base64.getEncoder().encodeToString()...
    public Date getExpirationDateFromToken(String token) {
        return getClaimFromToken(token, Claims::getExpiration);
    }

```

### Step 3: Create web security config

extend from `WebSecurityConfigurerAdapter`

```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    @Autowired
    private CustomUserDetailsService customUserDetailsService;
    @Autowired
    private JwtRequestFilter jwtRequestFilter;

// for authen: check user password
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(customUserDetailsService).passwordEncoder(customUserDetailsService.passwordEncoder());
    }

    @Override
    public void configure(WebSecurity web) throws Exception {
        web.ignoring().antMatchers(HttpMethod.POST, "/**/**");
        web.ignoring().antMatchers(HttpMethod.GET, "/**/**");
    }

// for author: check token if pass, check permission in url
// example: token has member role, then cannot access "member manager" page
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable()
            .authorizeRequests().antMatchers("/login","/account/**","/employee/**").permitAll()
            .antMatchers("/", "/home").access("hasRole('USER')")
            .antMatchers("/admin/**").hasRole("ADMIN")
            .anyRequest().authenticated()
            .and().formLogin()
            .and()
            .addFilterBefore(jwtRequestFilter, UsernamePasswordAuthenticationFilter.class);
    }

    @Bean
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }
}
```

### Step 4: Create a filter to catch request to check token

Need to extends `OncePerRequestFilter` after check token is valid? then save `UserDetails` to `SercurityContextHolder` then chain to destination of request.

```java

@Component
public class JwtRequestFilter extends OncePerRequestFilter {

    @Autowired
    private CustomUserDetailsService customUserDetailsService;
    @Autowired
    private JwtTokenUtils jwtTokenUtils;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
        String requireTokenHeader = request.getHeader("Authorization");
        String username = null;
        String token = null;

        System.out.println("run doFilterInternal");

        if (requireTokenHeader != null && requireTokenHeader.startsWith("Bearer ")) {
            token = requireTokenHeader.substring(7);
            try {
                // get username
                username = jwtTokenUtils.getUsernameFromToken(token);
            }  catch (IllegalArgumentException e) {
                System.out.println("Unable to get JWT Token");
            } catch (ExpiredJwtException e) {
                System.out.println("JWT Token has expired");
            }
            // Execute authen to check is username existence?
            System.out.println("Get username");
            UserDetails userDetails = customUserDetailsService.loadUserByUsername(username);
            UsernamePasswordAuthenticationToken auth = new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
            // save to Security context
            auth.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
            SecurityContextHolder.getContext().setAuthentication(auth);
        }
        // execute request
        filterChain.doFilter(request, response);
    }
}
```

<img src="blog/java/img/jwt7.png" style="display: block; margin-right: auto; margin-left: auto;">

### Step 5: (Optional) Create refresh token which use incase token is expired

```java 
    public RefreshToken createRefreshToken(Long userId) {
        RefreshToken refreshToken = new RefreshToken();

        refreshToken.setUser(userRepository.findById(userId).get());
        refreshToken.setExpiryDate(Instant.now().plusMillis(refreshTokenDurationMs));
        // or can create likt jwt
        refreshToken.setToken(UUID.randomUUID().toString());

        refreshToken = refreshTokenRepository.save(refreshToken);
        return refreshToken;
    }

    public RefreshToken verifyExpiration(RefreshToken token) {
        if (token.getExpiryDate().compareTo(Instant.now()) < 0) {
            refreshTokenRepository.delete(token);
            throw new TokenRefreshException(token.getToken(), "Refresh token was expired. Please make a new signin request");
        }

        return token;
    }
```

to create new token by refreshToken

```java
 @PostMapping("/refreshtoken")
    public ResponseEntity<?> refreshtoken(@Valid @RequestBody TokenRefreshRequest request) {
        String requestRefreshToken = request.getRefreshToken();

        return refreshTokenService.findByToken(requestRefreshToken)
            .map(refreshTokenService::verifyExpiration)
            .map(RefreshToken::getUser)
            .map(user -> {
                // return new token
                String token = jwtUtils.generateToken(user.getUsername());
                return ResponseEntity.ok(new TokenRefreshResponse(token, requestRefreshToken));
            })
            .orElseThrow(() -> new TokenRefreshException(requestRefreshToken,
                "Refresh token is not in database!"));
    }
```

# How to log out all logged-in user

To log out all users in Spring Security, you can use the `SessionRegistry` class. This class keeps track of all currently active sessions in the application. You can use the `getAllPrincipals()` method of this class to get a list of all currently logged-in users. You can then iterate over this list and call the `expireNow()` method of each session to log out the user².

Here is an example of how you can use the `SessionRegistry` class to log out all users in Spring Security²:

```java
@Autowired
private SessionRegistry sessionRegistry;

public void logoutAllUsers() {
    List<Object> principals = sessionRegistry.getAllPrincipals();

    for (Object principal : principals) {
        if (principal instanceof User) {
            List<SessionInformation> sessions = sessionRegistry.getAllSessions(principal, false);

            for (SessionInformation session : sessions) {
                session.expireNow();
            }
        }
    }
}
```

This code logs out all currently logged-in users by expiring their sessions.

I hope this helps! Let me know if you have any other questions.

Nguồn: Cuộc hội thoại với Bing, 5/31/2023
(1) How do you log out all logged in users in spring-security?. https://stackoverflow.com/questions/14751964/how-do-you-log-out-all-logged-in-users-in-spring-security.
(2) Handling Logouts :: Spring Security. https://docs.spring.io/spring-security/reference/servlet/authentication/logout.html.
(3) Logout :: Spring Security. https://docs.spring.io/spring-security/reference/reactive/authentication/logout.html.


# How to custom check OTP

```java
@Slf4j
@Component
public class JwtRequestFilter extends OncePerRequestFilter {

    @Autowired
    private UserDetailServiceImpl userDetailService;
    @Autowired
    private JwtTokenUtils jwtTokenUtils;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
        String requireTokenHeader = request.getHeader("Authorization");
        String token = null;

        System.out.println("run doFilterInternal");

        if (requireTokenHeader != null && requireTokenHeader.startsWith("Bearer ")) {
            token = requireTokenHeader.substring(7);
            Integer validate = jwtTokenUtils.validateToken(token);
            if (validate == 2) {
                log.debug("save user to security context holder");
                Authentication authentication = this.jwtTokenUtils.getAuthentication(token);
                SecurityContextHolder.getContext().setAuthentication(authentication);
            } else if (validate == 1) {
                // access normal
                log.debug("token valid");
            } else {
                return;
            }
        }
        filterChain.doFilter(request, response);
    }
}
```

```java
public Integer validateToken(String authToken)
    {
        try
        {
            Jwts.parser().setSigningKey(secretKey).parseClaimsJws(authToken);
//            if (SecurityContextHolder.getContext().getAuthentication().isAuthenticated()) return true;
            // parsing token. now, it does not use
            String accessToken = null;
            HttpServletRequest request = ((ServletRequestAttributes) Objects.requireNonNull(RequestContextHolder.getRequestAttributes())).getRequest();
            String bearerToken = request.getHeader(HttpHeaders.AUTHORIZATION);
            String[] uri = request.getRequestURI().split("/");
            if (StringUtils.hasText(bearerToken) && bearerToken.startsWith("Bearer ")) {
                accessToken = bearerToken.substring(7);
            }
            String[] parts = accessToken.split("\\.");
            if (parts.length < 1) {
                return 0;
            }
            String body = new String(Base64.getDecoder().decode(parts[1]));
            Gson gson = new Gson();
            JsonObject payLoad = gson.fromJson(body, JsonObject.class);
            Set<String> claims = payLoad.keySet();
            if (claims.contains("verifyToken") && uri[1].equals("verify")) {
                // if token for install otp then return 2 to save to `securityContextHolder`
                return 2;
            } else if (claims.contains("verifyToken") && !uri[1].equals("verify")) {
                // not do anythings when use token to fill in otp
                return 0;
            } else {
                // access normal
                return 1;
            }
        }
        catch (SignatureException e)
        {
            log.error("Invalid JWT signature: {}", e.getMessage());
            return 0;
        }
    }
```

# How to log out a special user


```java
@Autowired
private SessionRegistry sessionRegistry;

public void chooseUserSession(String username) {
    List<Object> principals = sessionRegistry.getAllPrincipals();

    for (Object principal : principals) {
        if (principal instanceof User) {
            // check session equals username
            if (((User) principal).getUsername().equals(username)) {
                List<SessionInformation> sessions = sessionRegistry.getAllSessions(principal, false);

                for (SessionInformation session : sessions) {
                    // Do something with the session
                }
            }
        }
    }
}
```



