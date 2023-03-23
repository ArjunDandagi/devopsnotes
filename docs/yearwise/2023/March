# Ingress 

- classic ELB is no more encouraged , don't use it 
- classic ELB doesn't support websockets 
- ALB has 2 mode of traffic proxy , instance mode(default) , IP mode (needs secondary ENI) 
- When using instance mode for the ingress to work service should be exposed as 'NodePort' 
- ingress healthchecks are by default checked for 2XX if the application returns 302,301 or 307 you need to annotate them to make it work for status_codes

[medium Blog on ALB Ingress with Amazon EKS](https://joachim8675309.medium.com/alb-ingress-with-amazon-eks-3d84cf822c85)
