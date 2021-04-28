# SVN MacOs(Bigsur 11.3 기준) 한글 이슈 해결방법

맥에서 한글로 된 파일 커밋 또는 윈도우에서 한글로 된 파일을 업데이트 했을 경우 자모 분리되는 경우가 있다.
이런 이유는 윈도우와 맥의 유니코드 정규화가 다르기 때문에 일어나는 현상이다.

## 해결 과정

구글링을 통해 몇가지 알아냈다.

SVN에 "--with-unicode-path" 유니코드를 지원하는 옵션을 추가하는 방법이다.<br>
하지만, 현재 버전에서는 "--with-unicode-path" 옵션이 없다.<br>
**SVN 1.17.14** 버전에 "--with-unicode-path"이 옵션이 있었고 해당 버전을 설치하려고 하였지만 실패했다.

```bash
$ brew tap homebrew/versions

Error: homebrew/versions was deprecated. This tap is now empty and all its contents were either deleted or migrated.
```

다음과 같은 에러로 homebrew/versions 기능이 homebrew-core 로 마이그레이션 되었다.
뿐만 아니라 **SVN 1.17.14** 버전은 MacOs Bigsur 기준에서 설치되지 않았다.

좀 더 구글링을 통해 다른 해결방안을 찾았다.
[참고](https://github.com/tholu/homebrew-tap)

## 해결 방법

```bash
$ brew tap tholu/tap
$ brew install --with-unicode-path tholu/tap/subversion18
$ brew link --overwrite subversion18
$ svn --version

svn, version 1.8.16 (r1740329)
compiled Apr 28 2021, 14:44:24 on x86_64-apple-darwin20.4.0

Copyright (C) 2016 The Apache Software Foundation.
This software consists of contributions made by many people;
see the NOTICE file for more information.
Subversion is open source software, see http://subversion.apache.org/

The following repository access (RA) modules are available:

* ra_svn : Module for accessing a repository using the svn network protocol.
  - with Cyrus SASL authentication
  - handles 'svn' scheme
* ra_local : Module for accessing a repository on local disk.
  - handles 'file' scheme
* ra_serf : Module for accessing a repository via WebDAV protocol using serf.
  - using serf 1.3.9
  - handles 'http' scheme
  - handles 'https' scheme
```

## 참조

https://github.com/paulirish/homebrew-versions-1

https://starmethod.tistory.com/1293

https://github.com/Homebrew/homebrew-core/tree/fdfa2a857440f51d393aa33206493bf1b49bf3a7

https://github.com/Homebrew/homebrew-core/blob/HEAD/Formula/subversion.rb

https://github.com/tholu/homebrew-tap
