# PowerShell Alias 설정

길이가 긴 명령어를 입력하기 번거로울 때 명령어를 축약하거나 다른 이름(별칭, alias)으로 호출하고 싶을 때 사용

## alias 설정 확인

```powershell
> Get-Alias
```

## alias 설정 하기

```powershell
> Set-Alias [별칭] [대상 명령]
```

```powershell
> Set-Alias kubectl k
> Set-Alias vi nvim
> sal kubectl k
```

## Get-Alias와 Set-Alias의 alias

```powershell
Alias           gal -> Get-Alias
Alias           sal -> Set-Alias
```



## 영구적 설정은 아님. 설정 유지시키기

위 방법으로 별칭을 설정할 수 있지만 PowerShell을 닫았다가 열면 다시 설정 초기화. 

PowerShell을 실행시킬 때마다 원하는 설정을 자동으로 진행하기 위해 프로파일이라는 파일을 읽어 초기 설정을 진행.

### 프로파일 경로 알아내기

```powershell
> $profile
```

이렇게 입력하면 프로파일 경로 출력

```powershell
C:\Users\(사용자명)\Documents\PowerShell\Microsoft.PowerShell_profile.ps1
```

저 파일에다가 위에서 입력한 alias 설정을 입력. 만약 저 경로와 파일이 보이지 않는다면 직접 생성.

### 프로파일 신규 생성시 주의 사항

```powershell
> notepad $profile
```

입력 후 새로 만들기

파일 내용은 아래와 같이 입력

```powershell
Set-Alias k kubectl.exe
```

PowerShell을 재시작 했을때 오류 발생.

```powershell
. : 이 시스템에서 스크립트를 실행할 수 없으므로 C:\Users\choro\OneDrive\문서\WindowsPowerShell\Microsoft.PowerShell_pro
file.ps1 파일을 로드할 수 없습니다. 자세한 내용은 about_Execution_Policies(https://go.microsoft.com/fwlink/?LinkID=1351
70)를 참조하십시오.
위치 줄:1 문자:3
+ . 'C:\Users\choro\OneDrive\문서\WindowsPowerShell\Microsoft.PowerShell_ ...
+   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : 보안 오류: (:) [], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
```

서명되지 않은 파워쉘 스크립트를 실행하려고 했기 때문

서명되지 않은 파워쉘 스크립트를 실행하려면 실행 규칙 정책을 변경 필요

### 정책 변경

#### 실행 규칙 정책 조회

```powershell
PS C:\> ExecutionPolicy
Restricted
```

Restricted(제한됨) 정책으로 확인

#### 실행 규칙 정책 변경

**관리자모드**로 PowerShell을 실행

```powershell
PS C:\> Set-ExecutionPolicy RemoteSigned
실행 규칙 변경 실행 정책은 신뢰하지 않는 스크립트로부터 사용자를 보호합니다. 실행 정책을 변경하면 about_Execution_Policies 도움말 항목(https://go.microsoft.com/fwlink/?LinkID=135170)에 설명된 보안 위험에 노출될 수 있습니다. 실행 정책을 변경하시겠습니까?
[Y] 예(Y) [A] 모두 예(A) [N] 아니요(N) [L] 모두 아니요(L) [S] 일시 중단(S) [?] 도움말 (기본값은 "N"): A
```

```powershell
PS C:\> ExecutionPolicy
RemoteSigned
```

이 후 PowerShell을 재시작하면 스크립트 정상 실행.

<br><br><br><br>

출처: https://jakupsil.tistory.com/47 [작업실 노트], https://itisguide.tistory.com/14