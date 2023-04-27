# amazon-cloudwatch-starter

## Amazon CloudWatch Logs
### 기능
> **로그 데이터 쿼리**   
> CloudWatch Logs Insights를 사용하여 로그 데이터를 대화식으로 검색하고 분석할 수 있습니다. 
> 운영상의 문제에 보다 효율적이고 효과적으로 대처할 수 있도록 쿼리를 수행할 수 있습니다. 
> `CloudWatch Logs Insights`에는 몇 가지 간단하지만 강력한 명령과 함께 특별히 구축된 쿼리 언어가 포함되어 있습니다.  
> 
> **Amazon EC2 인스턴스에서 로그 모니터링**   
> CloudWatch Logs를 사용하면 로그 데이터를 통해 애플리케이션과 시스템을 모니터링할 수 있습니다. 
> 예를 들어 CloudWatch Logs에서는 애플리케이션 로그에서 발생하는 오류의 수를 추적하고 오류 비율이 지정한 임계값을 초과할 때마다 알림을 전송할 수 있습니다.  
> CloudWatch Logs는 모니터링하는 데 로그 데이터를 사용하므로 코드를 변경할 필요가 없습니다. 
> 예를 들어 특정 리터럴 문자(예: "NullReferenceException")에 대한 애플리케이션 로그를 모니터링하거나 
> 로그 데이터의 특정 위치(예: Apache 액세스 로그의 "404" 상태 코드)에서 리터럴 문자의 출현 횟수를 계산할 수 있습니다. 
> 검색할 단어가 발견되면 CloudWatch Logs는 지정한 CloudWatch 지표로 데이터를 보고합니다.
> 
> **로그 보존**   
> 기본적으로 로그는 무기한으로 보존되며 만료되지 않습니다. 로그 그룹별로 보존 정책을 조정할 수 있고 무기한 보존 유지 또는 10년 및 하루 중 보존 기간을 선택합니다.

### 역할(role) 추가
> 기존 EC2 인스턴스에 연결된 역할(role)에 다음 정책 추가: `CloudWatchAgentServerPolicy`

### 통합 CloudWatch Agent 
> **에이전트 설치 및 설정**    
> ```shell
> # 에이전트 설치  
> sudo yum install -y amazon-cloudwatch-agent
> 
> # 에이전트 설정
> cd 
> vi config.json
> {
>   ...
> 	"logs": {
> 		"logs_collected": {
> 			"files": {
> 				"collect_list": [
> 					{
> 						"file_path": "<로그 파일 경로>",
> 						"log_group_name": "<application 이름>"
> 					}
> 				]
> 			}
> 		}
> 	}
>   ...
> }
> :wq
> 
> # 에이전트 실행
> sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
> -a fetch-config \
> -m ec2 \
> -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json -s
> ```

### 참조사이트
> [2. EC2 Linux Command Log 수집 - CW Agent](https://aws-diary.tistory.com/74)  

---

## CloudWatch Metrics
### TODO
> 

### 통합 CloudWatch Agent
> **에이전트 설치 및 설정**  
> ```shell
> # 에이전트 설치  
> sudo yum install -y amazon-cloudwatch-agent
> 
> # 에이전트 설정
> cd 
> vi config.json
> {
>   ...
>   TODO
>   ...
> }
> :wq
> 
> # 에이전트 실행
> sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
> -a fetch-config \
> -m ec2 \
> -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json -s
> ```

---

## IntelliJ IDEA 플러그인
### AWS Toolkit for IntelliJ IDEA
> 설치: TODO

### CloudWatch Log 보기
> TODO
