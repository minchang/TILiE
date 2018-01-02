# Mastering Chaos - A Netflix Guide to Microservices 
[발표 영상](https://www.youtube.com/watch?v=CZ3wIuvmHeM)

- 이 구조**에서의 문제점**은 **일주일 또는 이주일 간격으로 배포되는** Java Web 이라는 코드베이스가 모든 사람이 하나의 코드에서 작업 하는 **방식/의미인** 모놀리틱 서비스라는데 있습니다.
> The problem with this architecture was that the codebase for Java Web was monolithic in the sense that everbody was contributing to one codebase that got deployed on a weekly or bi-weekly basis.

- 여기에서 문제점은 **어떠한 문제가 발생하였을 때**, **원인을 규명하기가 어렵다**는 점입니다. 우리는 아마도 **일주일 넘게** 문제 해결에 매달렸을 겁니다. 
> The problem with that was when a change was introduced that caused the problem, it was difficult to diagnose. We probably spent well over a week troubleshooting.

- **하루 정도 지나** 문제가 발생하면, 몇군데 코드를 **뽑아내어/제거한뒤** 다시 돌려보고 어떤 일이 발생하는지 지켜봅니다. 
> It took about a day to happen, we tried to pulling out pieces of code, and running it again to see what would happen.

- 하나의 어플리케이션에 대해 매우 많은 변화들이 한꺼번에 **이루어졌기/적용되었기 때문에**, 앞선 과정이 **장기간에 걸쳐** 진행되었습니다.
> Because so many changes were rolling into that one application, this took an extended period of time.
