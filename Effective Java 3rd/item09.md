# try-finally 보다는 try-with-resources를 사용하라

- try-finally 의 경우 여러개의 자원을 사용하여 제대로 처리하기에는 너무 지저분
    - finally의 close 에서도 예외가 발생 가능하므로 해당 경우까지 예외처리 필요
- InputStream, java.sql.Connection 등의 명시적 close가 필요한 자원에 사용

```java
static String firstLineOfFile(String path, String defaultVal) {
    try (BufferedReader br = new BufferedReader(new FileReader(path))) {
        return br.readLine();
    } catch (IOException e) {
        return defaultVal;
    }
}

```