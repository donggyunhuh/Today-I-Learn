# 자바의 입출력 처리

### 자바프로그램은 파일의 InputStream 클래스를 읽어온다. read(x) 그리고 OutputStream 클래스로 쓴다 Write(y)

x, y 는 byte 또는 byte[]


내장 메모리 파일 처리
- 앱을 종료하고 다시 실행할 때 사용한 곳에서 이어서 작업하고 싶을 때 사용
- 내장 메모리의 저장 위치 :/data/data/패키지명/files 폴더

openFileOutPut()/openFIleOutput()으로 파일 열기 – fileOutputstream, inputstream 변환
read(), write()로 퍼알 앍기 쓰기
close로 파일 닫기

```xml
<LinearLayout>
<Button 
 android:id=“@+id/btnWrite”
 android:text=“내장 메모리에 파일 쓰기”/>

<Button 
 android:id =“@+id/btnRead”
 android:text = “내장메모리에 파일 쓰기”/>
</LinearLayout>
```

```java
Button btnRead, btnWrite;
btnRead = (Button)findViewById(R.id.btnRead);
btnWrite = (Button)findViewById(R.id.btnWrite);

btnWrite.setOnClickListener(new View.OnClickListener(){
  public void onClick(View){
    try(
           FileOutputStream fos = openFileOutPut(“file.txt”, Context.MODE_PRIVATE);            String str = “쿡북 안드로이드”;
          fos.write(str.getBytes());
          fos.close();
          Toast.makeText(getApplicatinContext(),“file.txt가 생성됨”, Toast.LENGTH_SHORT).show();  } }

catch (IOException e){

btnRead.setOnClickListener(new View.onClickListener(){ 
 public void onClick(View v){
FileInPutStream fis = openFileInput(“file.txt);
byte[] txt = new byte[30];

fis.read(txt);
String str =new String(txt); Toast.makeText(getApplicatinContext(),str, Toast.LENGTH_SHORT).show();  } }
        
}
```
