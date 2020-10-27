---
title: ajax로 controller에 list 보내기
date: 2020-10-27 23:30:36
category: Spring
thumbnail: { thumbnailSrc }
draft: false
---

Ajax로 Controller에 파라미터를 넘길때 json 형태로 보낸다.
<br>아래 예시는 tom이라는 사람의 나이는 21살이라는 데이터를 보낼때의 json 형태이다.

```json
{
  "name": "tom",
  "age": "21"
}
```

만약 처리해야 될 사람이 여러명이라면 list형태 전송할 수 있다.

```java script

var dataArray = [];
var data1 = {
   "name" : "tom",
   "age"  : "21"
}
var data2 = {
   "name" : "jack"
   "age"  : "22"
}
dataArray.push(data1);
dataArray.push(data2);
var paramList = {
   "paramList" : JSON.stringify(dataArray)
}

$.ajax({
         type : "POST",
         url : "/memberInfo.do",
         data : paramList,
         success : function(data) {},
         error : function(e) {}
      });
```

위와 같이 ajax에서 list형태의 json 데이터를 controller에 전송할 수 있다.
<br> controller에서는 아래와 같이 데이터를 받는다.

```java
import com.fasterxml.jackson.core.type.TypeReference;
import com.fasterxml.jackson.databind.ObjectMapper;
@RequestMapping("/memberInfo.do")
   public String memberList(@RequestParam Map<String, Object> parameters{
      String json = commandMap.get("paramList").toString();
      ObjectMapper mapper = new ObjectMapper();
      List<Map<String, Object>> paramList = mapper.readValue(json, new TypeReference<ArrayList<Map<String, Object>>>(){});
}
```

리스트를 DTO로 받아야할 경우는 아래와 같이 사용할 수 있다.

```java
List<dto> paramList = mapper.readValue(json, new TypeReference<ArrayList<dto>>(){});


```
