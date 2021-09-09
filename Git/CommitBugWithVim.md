# There was a problem with the editor 'vi'

> ## Reference
>
> 1. Stack Overflow - [Git-commit unable to open vim](https://stackoverflow.com/questions/26930924/git-commit-unable-to-open-vim)

<br/>



## 경위

특정 깃 커밋을 자세하게 작성해야 해서 `-m` 옵션을 사용하지 않고, vim을 통해 작성하고 저장했는데, 에러가 발생했다.

```bash
$ git commit
error: There was a problem with the editor 'vi'.
```

<br/>



## 원인

vi 에디터 자체의 버그라고 한다.

<br/>



## 해결

깃 설정 파일에 vim을 추가해주면 된다.

```shell
$ git config --global core.editor 'vim'
```

경우에 따라서는 vim의 절대 경로를 추가해주어야 할 수도 있다.

```bash
$ git config --global core.editor $(which vim)
```
