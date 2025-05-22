---
title: 클라우드 컴퓨팅
date: 2025-05-08
categories: [Cloud, Cloud Service]
tags: [Cloud, Cloud Service]
---

##  클라우드 컴퓨팅의 역사와 발전
<details>
    <summary><b>1965</b></summary>
    John McCarthy 가 <span style="color:#337ea9"><b>“미래의 컴퓨팅 환경은 공공시설을 사용하는 것과도 같은 것”</b></span> 이라는 개념 제시<br>
</details>

<details>
    <summary><b>1967</b></summary>
   가상화 기술의 핵심 요소인 <span style="color:#337ea9"><b>하이퍼바이저(Hypervisor)</b></span> 개발
    <blockquote class="prompt-info">
        <p>
            <b>하이퍼바이저(Hypervisor)</b><br>
            물리적 하드웨어 자원을 가상화하는 기술<br>
            가상머신의 생성부터 삭제까지 가상머신이 동작하는 모든 환경을 관리
        </p>
    </blockquote>
</details>

<details>
    <summary><b>1970</b></summary>
    네트워크망 다이어그램에서 <span style="color:#337ea9">클라우드</span>가 등장<br>
</details>

<details>
    <summary><b>1995, 3</b></summary>
    AT&T 가 클라우드 컴퓨팅 서비스 최초 시작
</details>  

<details>
    <summary><b>1996, 11</b></summary>
    <span style="color:#337ea9"><b>클라우드 컴퓨팅</b></span>이라는 용어를 공식적으로 사용
</details> 

<details>
    <summary><b>2006, 8</b></summary>
    아마존 EC2 등장
</details> 

<details>
    <summary><b>2008, 10</b></summary>
    마이크로소프트 Azure 등장
</details> 

<details>
    <summary><b>2010, 7</b></summary>
    클라우드 컴퓨팅을 누구나 사용할 수 있는 오픈 플랫폼으로 만드는 오픈 스택 프로젝트 시작
</details> 

<details>
    <summary><b>2012, 6</b></summary>
    오라클 클라우드 등장
</details> 

<details>
    <summary><b>2013</b></summary>
    도커(Docker) 발표
    <blockquote class="prompt-info">
        <p>
            <b>도커(Docker)</b><br>
            운영체제에 가상화된 애플리케이션 컨테이너를 생성, 배포 관리하는 오픈 소스 플랫폼
        </p>
    </blockquote>
</details>  
<br>
__<span style="background-color: #fff5b1">클라우드 컴퓨팅 발전이 가능했던 이유?</span>__  
__1. 유연한 컴퓨팅 시스템의 필요성 증가__  
    2008년 전세계 금융 위기로 경기 불황과 기업들의 저성장 기조로 비용 절감의 위기의식 ↑  
    ⇒ 많은 비용이 소요되는 데이터 센터나 전산실을 운영하는 것에 대한 부담으로 클라우드 시장이 위축됨  
    이후, 스마트폰의 보급으로 언제 어디서나 인터넷 접속이 가능해진 모바일 환경이 만들어지고 데이터양 기하급수적↑  

__2. 인식의 전환__  
    IT 자원을 소유해야 한다는 인식에서 대여한다는 것으로 전환됨  
    
__3. 기술의 발전__  
    2000년대에 들어 가상화, 분산처리, 데이터베이스 기술이 급속히 발전  
    ⇒ 개념으로만 존재하던 클라우드 컴퓨팅이 실현될 수 O  

##  클라우드 컴퓨팅이란?
네트워크(인터넷)를 통해 IT 자원을 제공하고 그 자원을 필요할 때마다 사용할 수 있도록 하는 기술  
CPU, 메모리, 스토리지, 네트워크 등의 컴퓨팅 리소스 제공  

__<span style="background-color: #fff5b1">클라우드</span>__  
\- 존재는 하지만 복잡하면서 굳이 알지 않아도 되는 것을 구름으로 추상화

##  클라우드 컴퓨팅의 특징
- __On Demand__  
    컴퓨팅 자원을 서비스 형태로 제공 받아 원할 때 원하는 만큼 사용하고 사용한 만큼만 요금 지불  
    <blockquote class="prompt-info">
        <p>
            <b>컴퓨팅 자원(리소스)을 서비스 형태로 제공 받아야 하는 이유</b><br>
            수요에 따라 자원을 탄력적으로 사용하여 효율적인 작업을 하기 위해<br>  
            ex) 트래픽 몰릴 때 - 서버 1000대 임대 / 트래픽 없을 때 - 서버 900대 반납, 나머지 100대만 사용
        </p>
    </blockquote>

- __대규모 확장성__  
    클라우드 사업자가 미리 구축한 __<span style="color:#337ea9">대규모 컴퓨팅 자원을 수요에 따라 확장해서 사용 가능</span>__  
    빅 테크들은 전세계 주요 도시에 컴퓨팅 자원을 구축해놓음 

- __종량제 과금__  
    __<span style="color:#337ea9">서비스를 사용한 만큼만 요금 지불</span>__  

- __관리 편의성__  
    클라우드 사업자가 제공하는 __<span style="color:#337ea9">컴퓨팅 자원 관리 기능을 사용할 수 있어 관리가 용이해짐</span>__  
    전통적으로 컴퓨팅 자원을 관리하기 위해서는 각 영역에 대한 전문가 필요했음  

## 클라우드 컴퓨팅의 분류 기준
- __Service Model (서비스 모델)__  
    제공되는 클라우드 서비스 형태에 대한 모델
    <table>
        <thead>
            <tr>
                <th></th>
                <th>On Premise</th>
                <th>IaaS (Infrastructure as a Service)</th>
                <th>PaaS (Platform as a Service)</th>
                <th>SaaS (Software as a Service)</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td><b>서비스</b></td>
                <td>IT 자원을 알아서 관리하는 것</td>
                <td>기본 IT 자원만 제공하는 서비스</td>
                <td>IaaS 에 개발 환경을 구축해 제공하는 서비스</td>
                <td>모든 기능이 동작하는 소프트웨어를 제공하는 서비스</td>
            </tr>
            <tr>
                <td><b>관리할 리소스</b></td>
                <td>
                    - Applications<br>
                    - Data<br>
                    - Runtime<br>
                    - Middleware<br>
                    - O/S<br>
                    - Virtualization<br>
                    - Servers<br>
                    - Storage<br>
                    - Networking<br>
                </td>
                <td>
                    - Applications<br>
                    - Data<br>
                    - Runtime<br>
                    - Middleware<br>
                    - O/S<br>
                </td>
                <td>
                    - Applications<br>
                    - Data<br>
                </td>
                <td>X</td>
            </tr>
            <tr>
                <td><b>제공받는 리소스</b></td>
                <td>X</td>
                <td>
                    - Virtualization<br>
                    - Servers<br>
                    - Storage<br>
                    - Networking<br>
                </td>
                <td>
                    - Runtime<br>
                    - Middleware<br>
                    - O/S<br>
                    - Virtualization<br>
                    - Servers<br>
                    - Storage<br>
                    - Networking<br>
                </td>
                <td>
                    - Applications<br>
                    - Data<br>
                    - Runtime<br>
                    - Middleware<br>
                    - O/S<br>
                    - Virtualization<br>
                    - Servers<br>
                    - Storage<br>
                    - Networking<br>
                </td>
            </tr>
            <tr>
                <td><b>예</b></td>
                <td></td>
                <td>AWS EC2, Azure VM 등</td>
                <td>AWS Elastic Beanstalk, Google App Engine 등</td>
                <td>구글 드라이브, 웹 메일, Office 365 등</td>
            </tr>
        </tbody>
    </table>
\* Runtime : 프로그램이 동작하는 환경 (JDK, Python 인터프리터 등)  
\* Middleware : RDBMS 가 설치된 형태

- __Deployment Model (배포 모델)__  
    실제로 클라우드를 어떻게 구축하는지에 대한 모델
    <table>
        <thead>
            <tr>
                <th></th>
                <th>Public Cloud</th>
                <th>Private Cloud</th>
                <th>Hybrid Cloud</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td><b>서비스</b></td>
                <td>클라우드 사업자가 제공하는 IT 자원 사용</td>
                <td>제한된 네트워크 상에서 특정 기업/사용자를 위한 클라우드 컴퓨팅 환경 구축하여 사용</td>
                <td>
                    Public 과 Private 클라우드를 조합해서 사용<br>
                    - 보안이 중요한 시스템은 Private, 그 외는 Public 으로 사용<br>
                    - 주로 Private 사용, 예상치 못한 트래픽이 몰릴 때는 Public 으로 확장<br>
                </td>
            </tr>
            <tr>
                <td><b>사용</b></td>
                <td>모든 사용자</td>
                <td>특정 사용자</td>
                <td></td>
            </tr>
            <tr>
                <td><b>특징</b></td>
                <td>
                    - 효율적인 비용<br>
                    - 관리에 들이는 시간↓<br>
                    - 제공받는 서비스에 종속적<br>
                </td>
                <td>
                    - 안전성, 보안성, 유연성↑<br>
                    - 구축 난이도 ↑<br>
                    - 유지보수 비용 부담↑<br>
                </td>
                <td>
                    - 확장성, 유연성↑<br>
                    - 복잡성↑ 👉 불필요한 지출 발생<br>
                </td>
            </tr>
        </tbody>
    </table>

## 클라우드 컴퓨팅에서 사용되는 주요 용어
- __데이터 센터(Data Center)__  
    수많은 서버들을 한데 모아 네트워크로 연결해 IT 자원을 제공하는 시설  
    IDC(Internet Data Center),  CDC(Cloud Data Center) 등으로 불림  
    발열 관리가 중요!
    
    __<span style="background-color: #fff5b1">서버 구성</span>__  
    - Rack - 서버/네트워크 장비들이 들어가는 프레임  
    - Server - Rack  안에 들어가는 컴퓨터(서버)  
    👉 이러한 구조로 되어있어 공간을 효율적으로 사용 가능함!
  

- __지역(Region)__  
    데이터 센터가 위치한 지역(국가/도시)  
    
    - 전세계 데이터 센터의 IT 리소스를 생성할 수 O  
    - 지역 선택은 서비스 성능에 큰 영향을 미침  
            서비스 할 고객의 지역과 자원 생성할 지역이 최대한 가까워야 빠른 네트워크로 서비스 가능  
    - 지역마다 자원 사용 비용이 상이함  
  

- __가용성 영역(Availablity Zone)__  
    고가용성을 위해 지역 내에 분산된 데이터 센터  
    하나의 지역은 2개 이상의 가용 영역(시스템이 정상적으로 가동되는 영역)으로 구성됨  
    가용 영역끼리는 전용 회선으로 빠른 네트워크 통신을 함  
    __<span style="color:#337ea9">하나의 지역 내에서 다수의 가용 영역에 서비스를 분산하면 가용성↑</span>__
  

- __가상화(Virtualization)__  
    소프트웨어로 가상의 하드웨어(VM)를 생성하는 기술  
    클라우드 서비스에서 서버를 사용할 때, 가상화된 서버를 제공받음  

## 주요 클라우드 사업자
- Amazon Web Service(AWS)
- Azure
- Google Cloud Platform(GCP)
- Naver Cloud Platform
- Alibaba/Tencent Cloud

## 참고
[클라우드컴퓨팅 기초부터 AWS 실습까지](https://youtube.com/playlist?list=PLmv2d328i1Q4ZK_7XQYB5SeMqNA4q_Ptq&si=UKd5G6Nwd4WQIHoY)  