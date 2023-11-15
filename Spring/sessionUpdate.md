### 닉네임 변경시 인증 세션 업데이트

``` java
@PostMapping("/profilePage/update/updateNickname")
public ResponseEntity<?> updateNickname(@RequestBody UserEditInfo userEditInfo) {
try {
UserEditInfo updatedUserEditInfo = userEditService.updateUserNickname(userEditInfo);

        // 사용자 인증 정보 업데이트
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
        if (authentication != null && authentication.getPrincipal() instanceof UserInfo) {
            UserInfo currentUser = (UserInfo) authentication.getPrincipal();
            currentUser.setUserNickNm(updatedUserEditInfo.getUserNickNm());
            // 필요한 경우 여기에서 추가 정보를 업데이트

            // 인증 정보를 SecurityContext에 다시 설정
            Authentication newAuth = new UsernamePasswordAuthenticationToken(currentUser, authentication.getCredentials(), authentication.getAuthorities());
            SecurityContextHolder.getContext().setAuthentication(newAuth);
        }

        return ResponseEntity.ok(updatedUserEditInfo);

    } catch (Exception e) {
        return ResponseEntity.internalServerError().body("An error occurred while updating the nickname: " + e.getMessage());
    }
}
``` 

### 확인버튼 누르면 닉네임 변경하는 js

코드 동작 순서
post로 json 파일을 보냄 - 컨트롤러가 세션 인증 정보 업데이트 - 컨트롤러에서 현재 세션의 userNo를 가져오고 이 userNo를 이용해
데이터베이스에서 userNickN을 찾음 - 타임리프 이용하여 동적 처리해 html에 보여줌

```javascript

$(document).ready(function() {

    var csrfToken = $("meta[name='_csrf']").attr("content");
    var csrfHeader = $("meta[name='_csrf_header']").attr("content");

    // 모든 AJAX 요청에 CSRF 토큰을 헤더로 보내도록 설정
    $.ajaxSetup({
        beforeSend: function(xhr) {
            xhr.setRequestHeader(csrfHeader, csrfToken);
        }
    });

    const nicknameEditButton = $('#nicknameEditButton');
    const nicknameEditBox = $('#nicknameEditBox');
    const confirmButton = $('#confirmButton');
    const checkDuplicateButton = $('#checkDuplicateButton');
    const nicknameInput = $('#nicknameInput');

    nicknameEditButton.on('click', function() {
        // 닉네임 변경 버튼 숨기기
        nicknameEditButton.hide();

        // 중복 확인 박스와 확인 버튼 표시
        nicknameEditBox.css('display', 'flex');
        confirmButton.show();
    });




    checkDuplicateButton.on('click', function() {
        const newNickname = nicknameInput.val();

        if (newNickname === '') {
            alert('닉네임을 입력해주세요.');
            return;
        }

        // 여기서 $.ajax를 사용하여 닉네임 중복 확인 로직을 수행합니다.
        $.ajax({
            type: "GET",

            url: "/user/mypage/profile/check/checkNickNm", // 요청 URL
            data: { newNickname: newNickname },  // 전송할 데이터
            dataType: "json",  // 응답 데이터의 유형을 지정 (JSON 으로 받을 경우)
            success: function(response) {
                if (response.exists) {
                    alert("닉네임이 이미 존재합니다.");
                } else {
                    alert("사용 가능한 닉네임입니다.");
                }
            },
            error: function(error) {
                console.error(error);
                alert("오류가 발생했습니다. 다시 시도하세요.");
            }
        });
    });






   confirmButton.on('click', function() {
        // Reset everything to original state
        const newNickname = nicknameInput.val();
        nicknameEditBox.hide();
        confirmButton.hide();
        nicknameEditButton.show();

        const userEditInfo = {
            userNo: getUserNo(), // userNo를 얻는 함수를 호출
            userNickNm: newNickname // 입력한 새 닉네임
        };

        $.ajax({
            type: "POST",
            url: "/user/mypage/profile/profilePage/update/updateNickname",
            contentType: "application/json",
            data: JSON.stringify(userEditInfo),
            dataType: "json",
            success: function(response) {
                // 서버에서 응답으로 온 UserEditInfo 객체 사용
                alert('닉네임이 <' + response.userNickNm + '>으로 변경되었습니다.');
                console.log('Nickname updated to: ', response.userNickNm);
                // 페이지 내 닉네임을 표시하는 요소를 업데이트 할 수 있습니다.
                $('#userNicknameDisplay').text(response.userNickNm);
                nicknameEditBox.hide();
                confirmButton.hide();
                nicknameEditButton.show();
            },or: function(xhr, status, error) {
                // 오류 메시지를 표시
                alert("An error occurred: " + xhr.responseText);
            }
        });
    });


});
```


