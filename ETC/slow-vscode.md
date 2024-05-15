# slow vscode

## VSCODE 가 느려질 때

VSCODE로 node 프로젝트를 진행하다가, 파일이 많아진 것이 문제인지 타이핑하는 것이 느려졌다.

파일 검색은 물론, 파일을 띄우는 전체적인 VSCODE내의 동작들이 2박자 정도 느리게 반응했다.

```shell
cmd + shipt + p
```

검색

```shell
> config runtime arguments
```

아래 코드 추가

```json
"disable-hardware-acceleration": true
```

위의 설정 적용 후 느려진 것이 어느정도 해결되었다.

해당 파라미터 값의 적용은 GPU 가속 비활성화를 한 것이라고 하며, 느려지는 현상이 발생하지 않을 때는
설정하지 말라고 한다.

참고

- [How can I disable GPU rendering in Visual Studio Code](https://stackoverflow.com/questions/29966747/how-can-i-disable-gpu-rendering-in-visual-studio-code)
