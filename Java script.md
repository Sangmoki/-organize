Java script
=============================
## java script


## json

### JSON 문자열이란 ?
1. 단일 값
- {"키":"값", "키":"값"} 으로 사용하는 형식이고
2. 리스트로 나타낼 땐
- [{"키":"값", "키":"값"}, {"키":"값", "키":"값"}]

### JSON 사용시 꼭 외워야 할 것
- JSON.parse - 문자열로 변경할거냐
- JSON.stringify - 일반 문자열을 제이슨으로 변경할거냐

## ajax
### ajax 참고용

<!-- 그냥 function만 들어가면 페이지가 로드되고 난 후 스크립트를 실행하겠다.-->
    <!-- function(data)는 함수 실행했는데 data라는 함수를 또 실행하는 콜백함수다 -->
    <script>

        $(function(){
            $.ajax({
                url: "/menu/category",
                success: function(data){

            // 태그 형식의 값을 담을 때 $를 쓴다.
            // 해당 태그의 코드가 categoryCode인 정보값을 가져오라. 그럼 html의 id = categoryCode를 가져와 여기에 담아놓는다.
                    const $categoryCode = $("#categoryCode")

            // html코드로 인식하겠다.
                    $categoryCode.html(""); /* 자바스크립트의 innerHTML과 동일한 역할이다. */
                    // 값을 초기화 시켜준다.

                    for(let index in data){
                        // 위에 담아놓은 categoryCode에 생성된 option태그를 생성해 append(추가)
                        // value에 인덱스 번호에 해당하는 categoryCode와 categoryName을 받아
                        $categoryCode.append($("<option>").val(data[index].categoryCode)
                            .text(data[index].categoryName));
                        // 그려주겠다.
                    }
                },
                error: function(error){
                    console.log(xhr);
                }
            });
        });
    </script>