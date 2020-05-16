##  어플리케이션 배포
#### 단일 인스턴스 시작  
kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1  

#### 매니페스트로부터 리소스 생성
kubectl apply -f ./my-manifest.yaml  




## 어플리케이션 조회 
#### Name으로 정렬된 서비스의 목록 조회
kubectl get services --sort-by=.metadata.name  

#### 재시작 횟수로 정렬된 파드의 목록 조회
kubectl get pods --sort-by='.status.containerStatuses[0].restartCount'  

#### PersistentVolumes을 용량별로 정렬해서 조회
kubectl get pv --sort-by=.spec.capacity.storage  

#### app=casandra 레이블을 가진 모든 파드의 레이블 버전 조회
kubectl get pods --selector=app=casandra -o \
  jasonpath='{.items[* ].metadata.labels.version}'

#### 네임스페이스의 모든 실행 중인 파드를 조회
kubectl get pods --field0selecotr=status.phase=Running  

#### 모든 노드의 외부IP를 조회
kubectl get nodes - o jsonpath='{.items[* ].status.addresses[?(@.type=="ExternalIP")].address}'

#### 모든 파드의 레이블 조회
kubectl get pods --show-labels

#### pod에 들어있는 컨테이너의 bash session을 실행
kubectl exec -ti $POD_NAME bash




## 리소스 업데이트
#### 앱 버전 업데이트
kubectl set image deployments/frontend www=image:v2 ("frontend" 디플로이먼트의 "www" 컨테이너의 이미지를 업데이트(rolling))  

#### 업데이트 롤백
kubectl rollout undo deployments/kubernetes-bootcamp  

#### 라벨 붙이기
kubectl label pod $POD_NAME app=v1

## 앱 외부로 노출하기 (service)
#### service 만들고 노출시키기
kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080



## 앱 스케일링하기
#### Deployment에서 replica를 4로 scaleout 시키기
kubectl scale deployments/kubernetes-bootcamp --replicas=4

#### mysql이라는 디플로이먼트의 현재크기가 2인경우, mysql을 3으로 스케일
kubectl scale --current-replicas=2 --replicas=3 deployment/mysql



## 리소스 삭제
#### "baz", "foo"와 동일한 이름을 가진 파드와 서비스 삭제
kubectl delete pod,service baz foo

#### name=myLabel 라벨을 가진 파드와 서비스 삭제
kubectl delete pods,services -l name=myLabel