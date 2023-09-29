# ElasticSearch

Elasticsearch luôn nằm trong top 10 của DB-Engines Ranking - bảng xếp hạng các hệ thống quản lý cơ sở dữ liệu theo mức phổ biến (https://db-engines.com/en/ranking)

# A. Giới thiệu chung

Một số thông tin quan trọng về Elasticsearch:

- Là Search Engine #1 thế giới. Phát triển bằng Java, dựa trên Apache Lucense.
- Tiền thân là Compass - Java Search Engine Framework (2004).
- Được phát hành phiên bản đầu tiên năm 2010, thuộc công ty Elastic
- Là một trong những sản phẩm Open Source cùng Logstash, Kibana, Beats,..

<img src="blog/java/img/elasticSearch.png" style="display: block; margin-right: auto; margin-left: auto;">

Trong hình trên, chúng ta có :

- Beats: Công cụ truyền dữ liệu từ nguồn tới Logstash hoặc Elasticsearch (single-purpose data shippers).
- Logstash: Nơi xử lý, chuyển đổi dữ liệu và truyền về Elasticsearch
- Elasticsearch: Tâm điểm của chúng ta. Nơi lưu trữ tập trung dữ liệu của bạn để tìm kiếm và phân tích.
- Kibana: Công cụ phân tích, hiển thị dữ liệu từ Elasticsearch một cách trực quan dễ sử dụng. Ngoài ra mình cũng không rõ có phải theo trend là các công ty công nghệ nước ngoài thường gắn với động vật hay không, nhưng bộ 3 sản phẩm nổi tiếng nhất Elasticsearch - Logstash - Kibana hay được gọi tắt là ELK cũng là nai sừng tấm.


## Ưu điểm:

- Mã nguồn mở, hoàn toàn miễn phí, cộng đồng phát triển lớn.
- Tốc độ truy vấn nhanh, kể cả các truy vấn phức tạp.
- Hỗ trợ full-tex search: tách từ, tách câu, tạo chỉ mục cho dữ liệu.
- Tìm kiếm mờ (fuzzy), tự động hoàn thành (autocomplete) giúp tìm kết quả kể cả khi sai chính tả.
- Chạy trên server riêng, tương tác qua RESTful API, có thể tích hợp với hầu hết mọi hệ thống.
- Dữ liệu lưu dạng JSON, rất linh hoạt trong trường hợp dữ liệu thường xuyên thay đổi cấu trúc.
- Khả năng mở rộng, tính sẵn sàng cao do dựng theo mô hình cluster.

## Nhược điểm:

- Có đầy đủ các nhược điểm của NoSQL.
- Không phù hợp với dữ liệu phải chỉnh sửa nhiều.
- Không hỗ trợ transaction, không có ràng buộc quan hệ dẫn đến dữ liệu có thể sai.


# B. Hướng dẫn cài đặt

Để cài đặt trên docker ta viết file docker compose

**docker-compose.yaml**

    version: "3.0"
    services:
    elasticsearch:
        container_name: es-container
        image: docker.elastic.co/elasticsearch/elasticsearch:7.10.0
        environment:
        - xpack.security.enabled=false
        - "discovery.type=single-node"
        networks:
        - es-net
        ports:
        - 9200:9200
    kibana:
        container_name: kb-container
        image: docker.elastic.co/kibana/kibana:7.10.0
        environment:
        - ELASTICSEARCH_HOSTS=http://es-container:9200
        networks:
        - es-net
        depends_on:
        - elasticsearch
        ports:
        - 5601:5601
    networks:
    es-net:
        driver: bridge


Sau khi cài đặt truy cập: `http://localhost:9200`

<img src="blog/java/img/elasticSearch1.png" style="display: block; margin-right: auto; margin-left: auto;">

Ngoài ra, chúng ta đã cài thêm Kibana - một nền tảng open source phân tích, hiển thị dữ liệu từ Elasticsearch một cách trực quan dễ sử dụng. Kibara được cấu hình chạy ở đường dẫn `http://localhost:5601/`


https://viblo.asia/p/elasticsearch-zero-to-hero-2-co-che-hoat-dong-cua-elasticsearch-38X4ENMXJN2

https://viblo.asia/p/elasticsearch-zero-to-hero-3-tao-index-trong-elasticsearch-tu-co-ban-den-nang-cao-m2vJPx78JeK





















