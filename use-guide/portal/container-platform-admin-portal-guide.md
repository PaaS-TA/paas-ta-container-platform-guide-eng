### [Index](https://github.com/PaaS-TA/Guide-eng/blob/master/README.md) > [CP Use](/use-guide/Readme.md) > Operator Portal Use Guide

<br>

## Table of Contents

1. [Document Outline](#1)
    * [1.1. Purpose](#1-1)
    * [1.2. Range](#1-2)
2. [Container Platform Operator Portal Access](#2)
    * [2.1. Container Platform Operator Portal Log in](#2-1)    
3. [Container Platform Operator Portal Manual](#3)
    * [3.1. Container Platform Operator Portal Menu Configuration](#3-1)
    * [3.2. Container Platform Operator Portal Menu Description](#3-2)
    * [3.2.1. Overview](#3-2-1)
    * [3.2.1.1. Overview List Check](#3-2-1-1)
    * [3.2.1.2. Overview Namespace Change](#3-2-1-2)
    * [3.2.2. Clusters Menu](#3-2-2)
    * [3.2.2.1. Namespaces](#3-2-2-1)
    * [3.2.2.1.1. Namespace List Check](#3-2-2-1-1)
    * [3.2.2.1.2. Namespace Details Check](#3-2-2-1-2)
    * [3.2.2.1.3. Create Namespace](#3-2-2-1-3)
    * [3.2.2.1.4. Modify Namespace](#3-2-2-1-4)
    * [3.2.2.1.5. Delete Namespace](#3-2-2-1-5)
    * [3.2.2.2. Nodes](#3-2-2-2)
    * [3.2.2.2.1. Node List Check](#3-2-2-2-1)
    * [3.2.2.2.2. Node Details Check](#3-2-2-2-2)
    * [3.2.3. Workloads Menu](#3-2-3)
    * [3.2.3.1. Deployments](#3-2-3-1)
    * [3.2.3.1.1. Deployment List Check](#3-2-3-1-1)
    * [3.2.3.1.2. Deployment Details Check](#3-2-3-1-2)
    * [3.2.3.1.3. Create Deployment](#3-2-3-1-3)
    * [3.2.3.1.4. Modify Deployment](#3-2-3-1-4)
    * [3.2.3.1.5. Delete Deployment](#3-2-3-1-5)
    * [3.2.3.2. Pods](#3-2-3-2)
    * [3.2.3.2.1. Pod List Check](#3-2-3-2-1)
    * [3.2.3.2.2. Pod Details Check](#3-2-3-2-2)
    * [3.2.3.2.3. Create Pod](#3-2-3-2-3)
    * [3.2.3.2.4. Modify Pod](#3-2-3-2-4)
    * [3.2.3.2.5. Delete Pod](#3-2-3-2-5)
    * [3.2.3.3. Replica Sets](#3-2-3-3)
    * [3.2.3.3.1. Replica Set List Chek](#3-2-3-3-1)
    * [3.2.3.3.2. Replica Set Details Check](#3-2-3-3-2)
    * [3.2.3.3.3. Create Replica Set](#3-2-3-3-3)
    * [3.2.3.3.4. Modify Replica Set](#3-2-3-3-4)
    * [3.2.3.3.5. Delete Replica Set](#3-2-3-3-5)
    * [3.2.4. Services Menu](#3-2-4)
    * [3.2.4.1. Service List Check](#3-2-4-1)
    * [3.2.4.2. Service Details Check](#3-2-4-2)
    * [3.2.4.3. Create Service](#3-2-4-3)
    * [3.2.4.4. Modify Service](#3-2-4-4)
    * [3.2.4.5. Delete Service](#3-2-4-5)
    * [3.2.5. Storages Menu](#3-2-5)
    * [3.2.5.1. Persistent Volumes](#3-2-5-1)
    * [3.2.5.1.1. Persistent Volume List Check](#3-2-5-1-1)
    * [3.2.5.1.2. Persistent Volume Details Check](#3-2-5-1-2)
    * [3.2.5.1.3. Create Persistent Volume](#3-2-5-1-3)
    * [3.2.5.1.4. Modify Persistent Volume](#3-2-5-1-4)
    * [3.2.5.1.5. Delete Persistent Volume](#3-2-5-1-5)
    * [3.2.5.2. Persistent Volume Claims](#3-2-5-2)
    * [3.2.5.2.1. Persistent Volume Claim List Chek](#3-2-5-2-1)
    * [3.2.5.2.2. Persistent Volume Claim Details Check](#3-2-5-2-2)
    * [3.2.5.2.3. Create Persistent Volume Claim](#3-2-5-2-3)
    * [3.2.5.2.4. Modify Persistent Volume Claim](#3-2-5-2-4)
    * [3.2.5.2.5. Delete Persistent Volume Claim](#3-2-5-2-5)
    * [3.2.5.3. Storage Classes](#3-2-5-3)
    * [3.2.5.3.1. Storage Class List Check](#3-2-5-3-1)
    * [3.2.5.3.2. Storage Class Details Check](#3-2-5-3-2)
    * [3.2.5.3.3. Create Storage Class](#3-2-5-3-3)
    * [3.2.5.3.4. Modify Storage Class](#3-2-5-3-4)
    * [3.2.5.3.5. Delete Storage Class](#3-2-5-3-5)
    * [3.2.6. Managements Menu](#3-2-6)
    * [3.2.6.1. Users](#3-2-6-1)
    * [3.2.6.1.1. Cluster Operator Check](#3-2-6-1-1)  
    * [3.2.6.1.2. Cluster Operator Details Check](#3-2-6-1-2)  
    * [3.2.6.1.3. Normal User List Check](#3-2-6-1-3)  
    * [3.2.6.1.4. Normal User Details Check](#3-2-6-1-4)  
    * [3.2.6.1.5. Modify User](#3-2-6-1-5)  
    * [3.2.6.2. Roles](#3-2-6-2)  
    * [3.2.6.2.1. Role List Check](#3-2-6-2-1)
    * [3.2.6.2.2. Role Details Check](#3-2-6-2-2)
    * [3.2.6.2.3. Create Role](#3-2-6-2-3)
    * [3.2.6.2.4. Modify Role](#3-2-6-2-4)
    * [3.2.6.2.5. Delete Role](#3-2-6-2-5)
    * [3.2.6.3. Resource Quotas](#3-2-6-3)
    * [3.2.6.3.1. Resource Quota List Check](#3-2-6-3-1)
    * [3.2.6.3.2. Resource Quota Details Check](#3-2-6-3-2)
    * [3.2.6.3.3. Create Resource Quota](#3-2-6-3-3)
    * [3.2.6.3.4. Modify Resource Quota](#3-2-6-3-4)
    * [3.2.6.3.5. Delete Resource Quota](#3-2-6-3-5)
    * [3.2.6.4. Limit Ranges](#3-2-6-4)
    * [3.2.6.4.1. Limit Range List Check](#3-2-6-4-1)
    * [3.2.6.4.2. Limit Range Details Check](#3-2-6-4-2)
    * [3.2.6.4.3. Create Limit Range](#3-2-6-4-3)
    * [3.2.6.4.4. Modify Limit Range](#3-2-6-4-4)
    * [3.2.6.4.5. Delete Limit Range](#3-2-6-4-5)


<br>


# <div id='1'/> 1. Document Outline

## <div id='1-1'/> 1.1. Purpose
This document describes on the usage of the container platform operator portal.

## <div id='1-2'/> 1.2. Range
This document describes how to use the container platform operator portal based on the Windows environment.

<br>


# <div id='2'/> 2. Container Platform Operator Portal Access
The container platform operator portal can be accessed at the address below.<br>
For {K8S_MASTER_NODE_IP} value, enter **Kubernetes Master Node Public IP** value.

- Container Platform Operator Portal Access URI : **http://{K8S_MASTER_NODE_IP}:32703** <br>

<br>

## <div id='2-1'/> 2.1. Container Platform Operator Portal Log in
The initial information on accessing the container platform operator portal is as follows.
- Access to http://{K8S_MASTER_NODE_IP}:32703.   
- Log in to the container platform operator's portal by using the username : **admin** / password : **admin** account.

![IMG_004]

<br>

# <div id='3'/> 3. Container Platform Operator Portal Manual


## <div id='3-1'/> 3.1. Container Platform Operator Portal Menu Configuration
| <center>Menu</center> | <center>Classification</center> | <center>Description</center> |
| :--- | :--- | :--- |
|| Overview | Container Platform Dashboard |
| Clusters | Namespaces | Manages Namespaces Information |
|| Nodes | Manages Nodes Information |
| Workloads | Deployments | Manages Deployments Information |
|| Pods | Manages Pods Information |
|| Replica Sets | Manages Replica Sets Information |
| Services | Services | Manages Services Information |
| Storages | Persistent Volumes | Manages Persistent Volumes Information |
|| Persistent Volume Claims | Manages Persistent Volume Claims Information |
|| Storage Classes | Manages Storage Classes Information |
| Managements | Users | Manages User |
|| Roles | Manages Roles |
|| Resource Quotas | Manages Resource Quotas Information |
|| Limit Ranges | Manages Limit Ranges Information |

<br>

## <div id='3-2'/> 3.2. Container Platform Operator Portal Menu Description
This chapter describes the menu of the container platform operator portal.

### <div id='3-2-1'/> 3.2.1. Overview
#### <div id='3-2-1-1'/> 3.2.1.1. Overview List Chek
- Look up the number of Namespace, Deployment, Pod, User, and charts of Deployment, Pod, and ReplicaSet.

1. After logging in, the first screen that appears is Overview page.

![IMG_005]

#### <div id='3-2-1-2'/> 3.2.1.2. Overview Namespace Change
- Click Select Box at the top of the screen and select the desired Namespace from the full Namespace list.

1. When you select Namespace in the Select Box, the Overview information for the Namespace is queried.

![IMG_006]
![IMG_007]

### <div id='3-2-2'/> 3.2.2. Clusters Menu
#### <div id='3-2-2-1'/> 3.2.2.1. Namespaces
##### <div id='3-2-2-1-1'/> 3.2.2.1.1. Namespace List Check
1. Click Namespace in the Clusters menu to go to the Namespace list page.

![IMG_009]

##### <div id='3-2-2-1-2'/> 3.2.2.1.2. Namespace Details Check
1. Click the Namespace name in the Namespace list to go to the Namespace detail page.

#### Namespace Details Page
![IMG_010]

##### <div id='3-2-2-1-3'/> 3.2.2.1.3. Create Namespace 
- Can specify the appropriate **Operator od Namespace**, **Resource Quotas**, **Limit Ranges** on the Create Namespace page.

1. NClick the Create button in the Namespace list to go to the Namespace creation page.

#### Namespace List Page
![IMG_011]

#### Namespace Creating Page
- **Admin User** allows you to specify a corresponding Namespace administrator.

![IMG_012]
![IMG_013]
![IMG_014]
![IMG_015]
![IMG_016]

##### <div id='3-2-2-1-4'/> 3.2.2.1.4. Modify Namespace
- Can modify the appropriate **Operator of Namespace**, **Resource Quotas**, **Limit Ranges** on the Modify Namespace page.

1. Click the Modify button in Namespace Details to go to the Modify Namespace page.

#### Namespace Details Page
![IMG_017]

#### Namespace Modifying Page
- Can change the corresponding Namespace manager through the **Admin User**.

![IMG_018]
![IMG_019]

##### <div id='3-2-2-1-5'/> 3.2.2.1.5. Delete Namespace
1. Namespace 상세에서 삭제 버튼을 클릭하여 Namespace 삭제를 한다.

#### Namespace 상세 페이지
![IMG_020]

#### Namespace 삭제 팝업창
![IMG_021]
![IMG_022]

#### <div id='3-2-2-2'/> 3.2.2.2. Nodes
##### <div id='3-2-2-2-1'/> 3.2.2.2.1. Node 목록 조회
1. Clusters 메뉴의 Nodes를 클릭하여 Node 목록 페이지로 이동한다.

![IMG_023]

##### <div id='3-2-2-2-2'/> 3.2.2.2.2. Node 상세 조회
1. Node 목록에서 Node명을 클릭하여 Node 상세 페이지로 이동한다.
#### Node 상세 페이지

![IMG_024]

### <div id='3-2-3'/> 3.2.3. Workloads 메뉴
#### <div id='3-2-3-1'/> 3.2.3.1 Deployment
##### <div id='3-2-3-1-1'/> 3.2.3.1.1. Deployment 목록 조회
1. Workloads의 Deployments를 클릭하여 Deployment 목록 페이지로 이동한다.

![IMG_025]

##### <div id='3-2-3-1-2'/> 3.2.3.1.2. Deployment 상세 조회
1. Deployment 목록에서 Deployment명을 클릭하여 Deployment 상세 페이지로 이동한다.

#### Deployment 상세 페이지
![IMG_026]

##### <div id='3-2-3-1-3'/> 3.2.3.1.3. Deployment 생성
1. Deployment 목록에서 생성 버튼을 클릭할 시 Deployment 생성 팝업창이 뜬다.

#### Deployment 목록 페이지
![IMG_027]

#### Deployment 생성 팝업창
![IMG_028]

##### <div id='3-2-3-1-4'/> 3.2.3.1.4. Deployment 수정
1. Deployment 상세에서 수정 버튼을 클릭할 시 Deployment 수정 팝업창이 뜬다.

#### Deployment 상세 페이지
![IMG_029]

#### Deployment 수정 팝업창
![IMG_030]

##### <div id='3-2-3-1-5'/> 3.2.3.1.5. Deployment 삭제
1. Deployment 상세에서 삭제 버튼을 클릭할 시 Deployment가 삭제된다.

#### Deployment 상세 페이지
![IMG_031]

#### Deployment 삭제 팝업창
![IMG_032]

#### <div id='3-2-3-2'/> 3.2.3.2. Pods
##### <div id='3-2-3-2-1'/> 3.2.3.2.1. Pod 목록 조회
1. Workloads의 Pods를 클릭하여 Pod 목록 페이지로 이동한다.

![IMG_033]

##### <div id='3-2-3-2-2'/> 3.2.3.2.2. Pod 상세 조회
1. Pod 목록에서 Pod명을 클릭하여 Pod 상세 페이지로 이동한다.

#### Pod 상세 페이지
![IMG_034]

##### <div id='3-2-3-2-3'/> 3.2.3.2.3. Pod 생성
1. Pod 목록에서 생성 버튼을 클릭할 시 Pod 생성 팝업창이 뜬다.

#### Pod 목록 페이지
![IMG_035]

#### Pod 생성 팝업창
![IMG_036]

##### <div id='3-2-3-2-4'/> 3.2.3.2.4. Pod 수정
1. Pod 상세에서 수정 버튼을 클릭할 시 Pod 수정 팝업창이 뜬다.

#### Pod 상세 페이지
![IMG_037]

#### Pod 수정 팝업창
![IMG_038]

##### <div id='3-2-3-2-5'/> 3.2.3.2.5. Pod 삭제
1. Pod 상세에서 삭제 버튼을 클릭할 시 Pod 삭제가 삭제된다.

#### Pod 상세 페이지
![IMG_039]

#### Pod 삭제 팝업창
![IMG_040]


#### <div id='3-2-3-3'/> 3.2.3.3. Replica Sets
##### <div id='3-2-3-3-1'/> 3.2.3.3.1. Replica Set 목록 조회
1. Workloads의 Replica Sets을 클릭하여 Replica Set 목록 페이지로 이동한다.

![IMG_041]

##### <div id='3-2-3-3-2'/> 3.2.3.3.2. Replica Set 상세 조회
1. Replica Set 목록에서 Replica Set명을 클릭하여 Replica Set 상세 페이지로 이동한다.

#### Replica Set 상세 페이지
![IMG_042]

##### <div id='3-2-3-3-3'/> 3.2.3.3.3. Replica Set 생성
1. Replica Set 목록에서 생성 버튼을 클릭할 시 Replica Set 생성 팝업창이 뜬다.

#### Replica Set 목록 페이지
![IMG_043]

#### Replica Set  생성 팝업창
![IMG_044]

##### <div id='3-2-3-3-4'/> 3.2.3.3.4. Replica Set 수정
1. Replica Set 상세에서 수정 버튼을 클릭할 시 Replica Set 수정 팝업창이 뜬다.

#### Replica Set 상세 페이지
![IMG_045]

#### Replica Set 수정 팝업창
![IMG_046]

##### <div id='3-2-3-3-5'/> 3.2.3.3.5. Replica Set 삭제
1. Replica Set 상세에서 삭제 버튼을 클릭할 시 Replica Set이 삭제된다.

#### Replica Set 상세 페이지
![IMG_047]

#### Replica Set 삭제 팝업창
![IMG_048]

### <div id='3-2-4'/> 3.2.4. Services 메뉴
#### <div id='3-2-4-1'/> 3.2.4.1. Service 목록 조회
1. Services를 클릭하여 Service 목록 페이지로 이동한다.

![IMG_049]

#### <div id='3-2-4-2'/> 3.2.4.2. Service 상세 조회
1. Service 목록에서 Service명을 클릭하여 Service 상세 페이지로 이동한다.

#### Service 상세 페이지
![IMG_050]

#### <div id='3-2-4-3'/> 3.2.4.3. Service 생성
1. Service 목록에서 생성 버튼을 클릭할 시 Service 생성 팝업창이 뜬다.

#### Service 목록 페이지
![IMG_051]

#### Service 생성 팝업창
![IMG_052]

#### <div id='3-2-4-4'/> 3.2.4.4. Service 수정
1. Service 상세에서 수정 버튼을 클릭할 시 Service 수정 팝업창이 뜬다.

#### Service 상세 페이지
![IMG_053]

#### Service 수정 팝업창
![IMG_054]

#### <div id='3-2-4-5'/> 3.2.4.5. Service 삭제

1. Service 상세에서 삭제 버튼을 클릭할 시 Service가 삭제된다.

#### Service 상세 페이지
![IMG_055]

#### Service 삭제 팝업창
![IMG_056]

### <div id='3-2-5'/> 3.2.5. Storages 메뉴
#### <div id='3-2-5-1'/> 3.2.5.1. Persistent Volumes
##### <div id='3-2-5-1-1'/> 3.2.5.1.1. Persistent Volume 목록 조회
1. Storages의 Persistent Volumes를 클릭하여 Persistent Volume 목록 페이지로 이동한다.

![IMG_057]

##### <div id='3-2-5-1-2'/> 3.2.5.1.2. Persistent Volume 상세 조회
1. Persistent Volume 목록에서 Persistent Volume명을 클릭하여 Persistent Volume 상세 페이지로 이동한다.

#### Persistent Volume 상세 페이지
![IMG_058]

##### <div id='3-2-5-1-3'/> 3.2.5.1.3. Persistent Volumee 생성
1. Persistent Volume 목록에서 생성 버튼을 클릭할 시 Persistent Volume 생성 팝업창이 뜬다.

#### Persistent Volume 목록 페이지
![IMG_059]

#### Persistent Volume 생성 팝업창
![IMG_060]


##### <div id='3-2-5-1-4'/> 3.2.5.1.4. Persistent Volume 수정
1. Persistent Volume 상세에서 수정 버튼을 클릭할 시 Persistent Volume 수정 팝업창이 뜬다.

#### Persistent Volume 상세 페이지
![IMG_061]

#### Persistent Volume 수정 팝업창
![IMG_062]

##### <div id='3-2-5-1-5'/> 3.2.5.1.5. Persistent Volume 삭제
1. Persistent Volume 상세에서 삭제 버튼을 클릭 할 시 Persistent Volume이 삭제된다.

#### Persistent Volume 상세 페이지
![IMG_063]

#### Persistent Volume 삭제 팝업창
![IMG_064]

#### <div id='3-2-5-2'/> 3.2.5.2. Persistent Volume Claims
##### <div id='3-2-5-2-1'/> 3.2.5.2.1. Persistent Volume Claim 목록 조회
1. Storages의 Persistent Volume Claims를 클릭하여 Persistent Volume Claim 목록 페이지로 이동한다.

![IMG_065]

##### <div id='3-2-5-2-2'/> 3.2.5.2.2. Persistent Volume Claim 상세 조회
1. Persistent Volume Claim 목록에서 Persistent Volume Claim명을 클릭하여 Persistent Volume Claim 상세 페이지로 이동한다.

#### Persistent Volume Claim 상세 페이지
![IMG_066]

##### <div id='3-2-5-2-3'/> 3.2.5.2.3. Persistent Volumee Claim 생성
1. Persistent Volume Claim 목록에서 생성 버튼을 클릭할 시 Persistent Volume Claim 생성 팝업창이 뜬다.

#### Persistent Volume Claim 목록 페이지
![IMG_067]

#### Persistent Volume Claim 생성 팝업창
![IMG_068]

##### <div id='3-2-5-2-4'/> 3.2.5.2.4. Persistent Volume Claim 수정
1. Persistent Volume Claim 상세에서 수정 버튼을 클릭할 시 Persistent Volume Claim 수정 팝업창이 뜬다.

#### Persistent Volume Claim 상세 페이지
![IMG_069]

#### Persistent Volume Claim 수정 팝업창
![IMG_070]

##### <div id='3-2-5-2-5'/> 3.2.5.2.5. Persistent Volume Claim 삭제
1. Persistent Volume Claim 상세에서 삭제 버튼을 클릭할 시 Persistent Volume Claim이 삭제된다.

#### Persistent Volume Claim 상세 페이지
![IMG_071]

#### Persistent Volume Claim 삭제 팝업창
![IMG_072]

#### <div id='3-2-5-3'/> 3.2.5.3. Storage Classes
##### <div id='3-2-5-3-1'/> 3.2.5.3.1. Storage Class 목록 조회
1. Storages의 Storage Classes를 클릭하여 Storage Class 목록 페이지로 이동한다.
![IMG_073]

##### <div id='3-2-5-3-2'/> 3.2.5.3.2. Storage Class 상세 조회
1. Storage Class 목록에서 Storage Class명을 클릭하여 Storage Class 상세 페이지로 이동한다.

#### Storage Class 상세 페이지
![IMG_074]

##### <div id='3-2-5-3-3'/> 3.2.5.3.3. Storage Class 생성
1. Storage Class 목록에서 생성 버튼을 클릭할 시 Storage Class 생성 팝업창이 뜬다.

#### Storage Class 목록 페이지
![IMG_075]

#### Storage Class 생성 팝업창
![IMG_076]

##### <div id='3-2-5-3-4'/> 3.2.5.3.4. Storage Class 수정
1. Storage Class 상세에서 수정 버튼을 클릭할 시 Storage Class 수정 팝업창이 뜬다.

#### Storage Class 상세 페이지
![IMG_077]

#### Storage Class 수정 팝업창
![IMG_078]

##### <div id='3-2-5-3-5'/> 3.2.5.3.5. Storage Class 삭제
1. Storage Class 상세에서 삭제 버튼을 클릭할 시 Storage Class가 삭제된다.

#### Storage Class 상세 페이지
![IMG_079]

#### Storage Class 삭제 팝업창
![IMG_080]

### <div id='3-2-6'/> 3.2.6. Managements
#### <div id='3-2-6-1'/> 3.2.6.1. Users
##### <div id='3-2-6-1-1'/> 3.2.6.1.1. 클러스터 관리자 조회
1. Managements 메뉴의 Users를 선택하고 Administrator탭을 클릭하여 클러스터 관리자를 조회한다.

![IMG_081]

##### <div id='3-2-6-1-2'/> 3.2.6.1.2. 클러스터 관리자 상세 조회
1.  클러스터 관리자 User ID를 클릭하여 클러스터 관리자 상세 조회 페이지로 이동한다.

#### 클러스터 관리자 상세 조회 페이지
![IMG_082]

##### <div id='3-2-6-1-3'/> 3.2.6.1.3.  일반 사용자 목록 조회
1. Managements 메뉴의 Users를 선택하고 User탭을 클릭하여 사용자 목록을 조회한다.
  + Active 탭 : Namespace/Role이 할당된 사용자 목록 
  + Inactive 탭 : Namespace/Role이 비할당된 사용자 목록

![IMG_083]

##### <div id='3-2-6-1-4'/> 3.2.6.1.4. 일반 사용자 상세 조회
1.  일반 사용자 User ID를 클릭하여 일반 사용자 상세 조회 페이지로 이동한다.

####  일반 사용자 상세 조회 페이지
![IMG_084]

##### <div id='3-2-6-1-5'/> 3.2.6.1.5. User 수정
- User 수정 페이지에서 해당 사용자가 이용할 Namespace와 Role을 수정할 수 있다.

1. User 상세에서 수정 버튼을 클릭하여 User 수정 페이지로 이동한다.

#### User 상세 페이지
![IMG_086]

#### User 수정 페이지
![IMG_087]

- 사용자에게 지정할 Namespace와 Role을 선택한다. 

![IMG_088]
![IMG_089]
![IMG_090]

#### <div id='3-2-6-2'/> 3.2.6.2. Roles
##### <div id='3-2-6-2-1'/> 3.2.6.2.1. Role 목록 조회
1. Managements 메뉴의 Roles를 클릭하여 Role 목록 페이지로 이동한다.
![IMG_091]

##### <div id='3-2-6-2-2'/> 3.2.6.2.2. Role 상세 조회
1. Role 목록에서 Role명을 클릭하여 Role 상세 페이지로 이동한다.

#### Role 상세 페이지
![IMG_092]

##### <div id='3-2-6-2-3'/> 3.2.6.2.3. Role 생성
1. Role 목록에서 생성 버튼을 클릭할 시 Role 생성 팝업창이 뜬다.

#### Role 목록 페이지
![IMG_093]

#### Role 생성 팝업창
![IMG_094]

##### <div id='3-2-6-2-4'/> 3.2.6.2.4. Role 수정
1. Role 상세에서 수정 버튼을 클릭할 시 Role 수정 팝업창이 뜬다.

#### Role 상세 페이지
![IMG_095]

#### Role 수정 팝업창
![IMG_096]

##### <div id='3-2-6-2-5'/> 3.2.6.2.5. Role 삭제
1. Role 상세에서 삭제 버튼을 클릭할 시 Role이 삭제된다.

#### Role 상세 페이지
![IMG_097]

#### Role 삭제 팝업창
![IMG_098]

#### <div id='3-2-6-3'/> 3.2.6.3. Resource Quotas
##### <div id='3-2-6-3-1'/> 3.2.6.3.1. Resource Quota 목록 조회
1. Managements 메뉴의 Resource Quotas를 클릭하여 Resource Quota 목록 페이지로 이동한다.
![IMG_099]

##### <div id='3-2-6-3-2'/> 3.2.6.3.2. Resource Quota 상세 조회
1. Resource Quota 목록에서 Resource Quota명을 클릭하여 Resource Quota 상세 페이지로 이동한다.

#### Resource Quota 상세 페이지
![IMG_100]

##### <div id='3-2-6-3-3'/> 3.2.6.3.3. Resource Quota 생성
1. Resource Quota 목록에서 생성 버튼을 클릭할 시 Resource Quota 생성 팝업창이 뜬다.

#### Resource Quota 목록 페이지
![IMG_101]

#### Resource Quota 생성 팝업창
![IMG_102]

##### <div id='3-2-6-3-4'/> 3.2.6.3.4. Resource Quota 수정
1. Resource Quota 상세에서 수정 버튼을 클릭할 시 Resource Quota 수정 팝업창이 뜬다.

#### Resource Quota 상세 페이지
![IMG_103]

#### Resource Quota 수정 팝업창
![IMG_104]

##### <div id='3-2-6-3-5'/> 3.2.6.3.5. Resource Quota 삭제
1. Resource Quota 상세에서 삭제 버튼을 클릭할 시 Resource Quota가 삭제된다.

#### Resource Quota 상세 페이지
![IMG_105]

#### Resource Quota 삭제 팝업창
![IMG_106]

#### <div id='3-2-6-4'/> 3.2.6.4. Limit Ranges
##### <div id='3-2-6-4-1'/> 3.2.6.4.1. Limit Range 목록 조회
1. Managements 메뉴의 Limit Ranges를 클릭하여 Limit Range 목록 페이지로 이동한다.
![IMG_107]

##### <div id='3-2-6-4-2'/> 3.2.6.4.2. Limit Range 상세 조회
1. Limit Range 목록에서 Limit Range명을 클릭하여 Limit Range 상세 페이지로 이동한다.

#### Limit Range 상세 페이지
![IMG_108]

##### <div id='3-2-6-4-3'/> 3.2.6.4.3. Limit Range 생성
1. Limit Range 목록에서 생성 버튼을 클릭할 시 Limit Range 생성 팝업창이 뜬다.

#### Limit Range 목록 페이지
![IMG_109]

#### Limit Range 생성 팝업창
![IMG_110]

##### <div id='3-2-6-4-4'/> 3.2.6.4.4. Limit Range 수정
1. Limit Range 상세에서 수정 버튼을 클릭할 시 Limit Range 수정 팝업창이 뜬다.

#### Limit Range 상세 페이지
![IMG_111]

#### Limit Range 수정 팝업창
![IMG_112]

##### <div id='3-2-6-4-5'/> 3.2.6.4.5. Limit Range 삭제
1. Limit Range 상세에서 삭제 버튼을 클릭할 시 Limit Range가 삭제된다.

#### Limit Range 상세 페이지
![IMG_113]

#### Limit Range 삭제 팝업창
![IMG_114]

<br>

### [Index](https://github.com/PaaS-TA/Guide-eng/blob/master/README.md) > [CP Use](/use-guide/Readme.md) > 운영자 포털 사용 가이드

----
[IMG_001]:../images/v1.2/container-platform-portal/admin/cp-001.png
[IMG_002]:../images/v1.2/container-platform-portal/admin/cp-002.png
[IMG_003]:../images/v1.2/container-platform-portal/admin/cp-003.png
[IMG_004]:../images/v1.2/container-platform-portal/admin/cp-004.png
[IMG_005]:../images/v1.2/container-platform-portal/admin/cp-005.png
[IMG_006]:../images/v1.2/container-platform-portal/admin/cp-006.png
[IMG_007]:../images/v1.2/container-platform-portal/admin/cp-007.png
[IMG_008]:../images/v1.2/container-platform-portal/admin/cp-008.png
[IMG_009]:../images/v1.2/container-platform-portal/admin/cp-009.png
[IMG_010]:../images/v1.2/container-platform-portal/admin/cp-010.png
[IMG_011]:../images/v1.2/container-platform-portal/admin/cp-011.png
[IMG_012]:../images/v1.2/container-platform-portal/admin/cp-012.png
[IMG_013]:../images/v1.2/container-platform-portal/admin/cp-013.png
[IMG_014]:../images/v1.2/container-platform-portal/admin/cp-014.png
[IMG_015]:../images/v1.2/container-platform-portal/admin/cp-015.png
[IMG_016]:../images/v1.2/container-platform-portal/admin/cp-016.png
[IMG_017]:../images/v1.2/container-platform-portal/admin/cp-017.png
[IMG_018]:../images/v1.2/container-platform-portal/admin/cp-018.png
[IMG_019]:../images/v1.2/container-platform-portal/admin/cp-019.png
[IMG_020]:../images/v1.2/container-platform-portal/admin/cp-020.png
[IMG_021]:../images/v1.2/container-platform-portal/admin/cp-021.png
[IMG_022]:../images/v1.2/container-platform-portal/admin/cp-022.png
[IMG_023]:../images/v1.2/container-platform-portal/admin/cp-023.png
[IMG_024]:../images/v1.2/container-platform-portal/admin/cp-024.png
[IMG_025]:../images/v1.2/container-platform-portal/admin/cp-025.png
[IMG_026]:../images/v1.2/container-platform-portal/admin/cp-026.png
[IMG_027]:../images/v1.2/container-platform-portal/admin/cp-027.png
[IMG_028]:../images/v1.2/container-platform-portal/admin/cp-028.png
[IMG_029]:../images/v1.2/container-platform-portal/admin/cp-029.png
[IMG_030]:../images/v1.2/container-platform-portal/admin/cp-030.png
[IMG_031]:../images/v1.2/container-platform-portal/admin/cp-031.png
[IMG_032]:../images/v1.2/container-platform-portal/admin/cp-032.png
[IMG_033]:../images/v1.2/container-platform-portal/admin/cp-033.png
[IMG_034]:../images/v1.2/container-platform-portal/admin/cp-034.png
[IMG_035]:../images/v1.2/container-platform-portal/admin/cp-035.png
[IMG_036]:../images/v1.2/container-platform-portal/admin/cp-036.png
[IMG_037]:../images/v1.2/container-platform-portal/admin/cp-037.png
[IMG_038]:../images/v1.2/container-platform-portal/admin/cp-038.png
[IMG_039]:../images/v1.2/container-platform-portal/admin/cp-039.png
[IMG_040]:../images/v1.2/container-platform-portal/admin/cp-040.png
[IMG_041]:../images/v1.2/container-platform-portal/admin/cp-041.png
[IMG_042]:../images/v1.2/container-platform-portal/admin/cp-042.png
[IMG_043]:../images/v1.2/container-platform-portal/admin/cp-043.png
[IMG_044]:../images/v1.2/container-platform-portal/admin/cp-044.png
[IMG_045]:../images/v1.2/container-platform-portal/admin/cp-045.png
[IMG_046]:../images/v1.2/container-platform-portal/admin/cp-046.png
[IMG_047]:../images/v1.2/container-platform-portal/admin/cp-047.png
[IMG_048]:../images/v1.2/container-platform-portal/admin/cp-048.png
[IMG_049]:../images/v1.2/container-platform-portal/admin/cp-049.png
[IMG_050]:../images/v1.2/container-platform-portal/admin/cp-050.png
[IMG_051]:../images/v1.2/container-platform-portal/admin/cp-051.png
[IMG_052]:../images/v1.2/container-platform-portal/admin/cp-052.png
[IMG_053]:../images/v1.2/container-platform-portal/admin/cp-053.png
[IMG_054]:../images/v1.2/container-platform-portal/admin/cp-054.png
[IMG_055]:../images/v1.2/container-platform-portal/admin/cp-055.png
[IMG_056]:../images/v1.2/container-platform-portal/admin/cp-056.png
[IMG_057]:../images/v1.2/container-platform-portal/admin/cp-057.png
[IMG_058]:../images/v1.2/container-platform-portal/admin/cp-058.png
[IMG_059]:../images/v1.2/container-platform-portal/admin/cp-059.png
[IMG_060]:../images/v1.2/container-platform-portal/admin/cp-060.png
[IMG_061]:../images/v1.2/container-platform-portal/admin/cp-061.png
[IMG_062]:../images/v1.2/container-platform-portal/admin/cp-062.png
[IMG_063]:../images/v1.2/container-platform-portal/admin/cp-063.png
[IMG_064]:../images/v1.2/container-platform-portal/admin/cp-064.png
[IMG_065]:../images/v1.2/container-platform-portal/admin/cp-065.png
[IMG_066]:../images/v1.2/container-platform-portal/admin/cp-066.png
[IMG_067]:../images/v1.2/container-platform-portal/admin/cp-067.png
[IMG_068]:../images/v1.2/container-platform-portal/admin/cp-068.png
[IMG_069]:../images/v1.2/container-platform-portal/admin/cp-069.png
[IMG_070]:../images/v1.2/container-platform-portal/admin/cp-070.png
[IMG_071]:../images/v1.2/container-platform-portal/admin/cp-071.png
[IMG_072]:../images/v1.2/container-platform-portal/admin/cp-072.png
[IMG_073]:../images/v1.2/container-platform-portal/admin/cp-073.png
[IMG_074]:../images/v1.2/container-platform-portal/admin/cp-074.png
[IMG_075]:../images/v1.2/container-platform-portal/admin/cp-075.png
[IMG_076]:../images/v1.2/container-platform-portal/admin/cp-076.png
[IMG_077]:../images/v1.2/container-platform-portal/admin/cp-077.png
[IMG_078]:../images/v1.2/container-platform-portal/admin/cp-078.png
[IMG_079]:../images/v1.2/container-platform-portal/admin/cp-079.png
[IMG_080]:../images/v1.2/container-platform-portal/admin/cp-080.png
[IMG_081]:../images/v1.2/container-platform-portal/admin/cp-081.png
[IMG_082]:../images/v1.2/container-platform-portal/admin/cp-082.png
[IMG_083]:../images/v1.2/container-platform-portal/admin/cp-083.png
[IMG_084]:../images/v1.2/container-platform-portal/admin/cp-084.png
[IMG_085]:../images/v1.2/container-platform-portal/admin/cp-085.png
[IMG_086]:../images/v1.2/container-platform-portal/admin/cp-086.png
[IMG_087]:../images/v1.2/container-platform-portal/admin/cp-087.png
[IMG_088]:../images/v1.2/container-platform-portal/admin/cp-088.png
[IMG_089]:../images/v1.2/container-platform-portal/admin/cp-089.png
[IMG_090]:../images/v1.2/container-platform-portal/admin/cp-090.png
[IMG_091]:../images/v1.2/container-platform-portal/admin/cp-091.png
[IMG_092]:../images/v1.2/container-platform-portal/admin/cp-092.png
[IMG_093]:../images/v1.2/container-platform-portal/admin/cp-093.png
[IMG_094]:../images/v1.2/container-platform-portal/admin/cp-094.png
[IMG_095]:../images/v1.2/container-platform-portal/admin/cp-095.png
[IMG_096]:../images/v1.2/container-platform-portal/admin/cp-096.png
[IMG_097]:../images/v1.2/container-platform-portal/admin/cp-097.png
[IMG_098]:../images/v1.2/container-platform-portal/admin/cp-098.png
[IMG_099]:../images/v1.2/container-platform-portal/admin/cp-099.png
[IMG_100]:../images/v1.2/container-platform-portal/admin/cp-100.png
[IMG_101]:../images/v1.2/container-platform-portal/admin/cp-101.png
[IMG_102]:../images/v1.2/container-platform-portal/admin/cp-102.png
[IMG_103]:../images/v1.2/container-platform-portal/admin/cp-103.png
[IMG_104]:../images/v1.2/container-platform-portal/admin/cp-104.png
[IMG_105]:../images/v1.2/container-platform-portal/admin/cp-105.png
[IMG_106]:../images/v1.2/container-platform-portal/admin/cp-106.png
[IMG_107]:../images/v1.2/container-platform-portal/admin/cp-107.png
[IMG_108]:../images/v1.2/container-platform-portal/admin/cp-108.png
[IMG_109]:../images/v1.2/container-platform-portal/admin/cp-109.png
[IMG_110]:../images/v1.2/container-platform-portal/admin/cp-110.png
[IMG_111]:../images/v1.2/container-platform-portal/admin/cp-111.png
[IMG_112]:../images/v1.2/container-platform-portal/admin/cp-112.png
[IMG_113]:../images/v1.2/container-platform-portal/admin/cp-113.png
[IMG_114]:../images/v1.2/container-platform-portal/admin/cp-114.png