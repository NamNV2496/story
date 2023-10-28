# Swagger


## ============================= WAY 1 Can conflict =============================

<img src="blog/java/img/swagger1.png" style="display: block; margin-right: auto; margin-left: auto;">

Config to enable swagger UI

<img src="blog/java/img/swagger3.png" style="display: block; margin-right: auto; margin-left: auto;">


## ============================= WAY 2 =============================

<img src="blog/java/img/swagger4.png" style="display: block; margin-right: auto; margin-left: auto;">

### Enable swagger 2

<img src="blog/java/img/swagger2.png" style="display: block; margin-right: auto; margin-left: auto;">

### Add a config in `yml` file
```yml
spring:
  mvc:
    pathmatch:
      matching-strategy: ant_path_matcher

OR

spring.mvc.pathmatch.matching-strategy=ANT_PATH_MATCHER
```

# Access to `http://localhost:8080/swagger-ui/`

## ============================= WAY 3 =============================

Sử dụng openAPI

<img src="blog/java/img/swagger5.png" style="display: block; margin-right: auto; margin-left: auto;">

### Enable swagger 2

<img src="blog/java/img/swagger2.png" style="display: block; margin-right: auto; margin-left: auto;">

# Access to `http://localhost:8080/swagger-ui/index.html`

## ============================= Result =============================

<img src="blog/java/img/swagger6.png" style="display: block; margin-right: auto; margin-left: auto;">

