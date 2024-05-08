Java Rest api 샘플 소스
이 가디으는 Spring을 사용하여 RESTFul 웹 서비스를 생성하는 과정을 안내합니다.

구축 방법
HTTP GET 요청을 수락하는 서비스를 구축한다. http://localhost8080/api/simple 다음과 같은 JSON으로 표현하여 응답한다.

[
  {
    "num": 1,
    "title": "test-0",
    "content": "contents-0"
  },
  {
    "num": 2,
    "title": "test-1",
    "content": "contents-1"
  },
  {
    "num": 3,
    "title": "test-2",
    "content": "contents-2"
  },
  {
    "num": 4,
    "title": "test-3",
    "content": "contents-3"
  },
  {
    "num": 5,
    "title": "test-4",
    "content": "contents-4"
  },
  {
    "num": 6,
    "title": "test-5",
    "content": "contents-5"
  },
  {
    "num": 7,
    "title": "test-6",
    "content": "contents-6"
  },
  {
    "num": 8,
    "title": "test-7",
    "content": "contents-7"
  },
  {
    "num": 9,
    "title": "test-8",
    "content": "contents-8"
  },
  {
    "num": 10,
    "title": "test-9",
    "content": "contents-9"
  }
]
준비
약 15분
즐겨 사용하는 IDE
Java 17 이상
Maven
클래스 생성
서비스는 선택적으로 문자열의 매개변수를 사용하여 GET 에 대한 요청을 처리합니다. 요청은 JSON이 포한된 응답을 반환합니다.

[
  {
    "num": 1,
    "title": "test-0",
    "content": "contents-0"
  },
  {
    "num": 2,
    "title": "test-1",
    "content": "contents-1"
  },
  {
    "num": 3,
    "title": "test-2",
    "content": "contents-2"
  },
  {
    "num": 4,
    "title": "test-3",
    "content": "contents-3"
  },
  {
    "num": 5,
    "title": "test-4",
    "content": "contents-4"
  },
  {
    "num": 6,
    "title": "test-5",
    "content": "contents-5"
  },
  {
    "num": 7,
    "title": "test-6",
    "content": "contents-6"
  },
  {
    "num": 8,
    "title": "test-7",
    "content": "contents-7"
  },
  {
    "num": 9,
    "title": "test-8",
    "content": "contents-8"
  },
  {
    "num": 10,
    "title": "test-9",
    "content": "contents-9"
  }
]
피드 num, title, content 표현된다.

package com.example.javademoapi.model;
import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import lombok.ToString;

@Getter
@Setter
@ToString
@AllArgsConstructor
@NoArgsConstructor
public class Simple {
	
	private int num;
	private String title;
	private String content;
}
리소스 컨트롤러 생성
Spring의 RESTful 웹 서비스 빌딩 방식에서 HTTP 요청은 컨트롤러에 의해 처리됩니다. 이러한 컴포넌트들은 @RestController 어노테이션에 의해 식별되며, 다음 목록에 나와 있는 SimpleApiController 는 /api/simple에 대한 GET 요청을 처리하고 Simple 클래스의 새 인스턴스를 반환합니다.

package com.example.javademoapi.controller;
import java.util.ArrayList;
import java.util.List;

import com.example.javademoapi.model.Simple;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import lombok.extern.slf4j.Slf4j;

@RestController
@RequestMapping("/api")
@Slf4j
public class SimpleApiController {
    @GetMapping("/simple")
    public List<Simple> listSimple(){
        List<Simple> list = new ArrayList<>();

        for(int i=0 ; i< 10;i++) {
            list.add(new Simple(i+1,"test-"+i, "contents-"+i));
            log.info("for "+i);
        }
        log.info(list.toString());
        return list;

    }

}

서비스 실행
Spring Initializr는 애플리케이션 클래스를 생성합니다. 이 경우에는 클래스를 추가로 수정할 필요가 없습니다

package com.example.javademoapi;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class JavaDemoApiApplication {

    public static void main(String[] args) {
        SpringApplication.run(JavaDemoApiApplication.class, args);
    }

}

@SpringBootApplication은 다음을 모두 추가하는 편리한 어노테이션입니다.

@Configuration: 애플리케이션 컨텍스트의 빈 정의 소스로 클래스를 태그합니다.
@EnableAutoConfiguration: Spring Boot에게 클래스 패스 설정, 다른 빈 및 다양한 프로퍼티 설정을 기반으로 빈을 추가하기 시작하도록 알려줍니다. 예를 들어, spring-webmvc가 클래스 패스에 있으면 이 어노테이션은 애플리케이션을 웹 애플리케이션으로 플래그 지정하고 DispatcherServlet 설정과 같은 핵심 동작을 활성화합니다.
@ComponentScan: Spring에게 com/example 패키지에서 다른 컴포넌트, 설정 및 서비스를 찾도록 알려주어 컨트롤러를 찾을 수 있도록 합니다. main() 메서드는 Spring Boot의 SpringApplication.run() 메서드를 사용하여 애플리케이션을 시작합니다. XML 한 줄이 없는 것을 알아 보셨나요? web.xml 파일도 없습니다. 이 웹 애플리케이션은 100% 순수 자바이며 플러밍이나 인프라 구성에 대한 처리가 필요하지 않습니다.
서비스 테스트
http://localhost:8080/api/simple 다음을 확인할 수 있습니다.

[
  {
    "num": 1,
    "title": "test-0",
    "content": "contents-0"
  },
  {
    "num": 2,
    "title": "test-1",
    "content": "contents-1"
  },
  {
    "num": 3,
    "title": "test-2",
    "content": "contents-2"
  },
  {
    "num": 4,
    "title": "test-3",
    "content": "contents-3"
  },
  {
    "num": 5,
    "title": "test-4",
    "content": "contents-4"
  },
  {
    "num": 6,
    "title": "test-5",
    "content": "contents-5"
  },
  {
    "num": 7,
    "title": "test-6",
    "content": "contents-6"
  },
  {
    "num": 8,
    "title": "test-7",
    "content": "contents-7"
  },
  {
    "num": 9,
    "title": "test-8",
    "content": "contents-8"
  },
  {
    "num": 10,
    "title": "test-9",
    "content": "contents-9"
  }
]
