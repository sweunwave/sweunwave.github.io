---
layout: post
title:  "conda 가상 환경 사용법"
date:   2022-11-02 21:03:36 +0530
categories: Anaconda3
---
conda 가상환경 만들기

```conda create -n <환경명> python=버젼```

가상환경 리스트 확인

```conda env list```

가상환경 시작

```conda activate <환경명>```

가상환경 종료

```conda deactivate```

가상환경 패키지 설치

```conda install 패키지이름```

제거
**현재 위치가 (base) 임을 확인하고 삭제해야함!**
```conda remove -n <환경명> --all```
