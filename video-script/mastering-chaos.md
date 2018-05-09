# Mastering Chaos - A Netflix Guide to Microservices 
[발표 영상](https://www.youtube.com/watch?v=CZ3wIuvmHeM)

- 이 구조**에서의 문제점**은 **일주일 또는 이주일 간격으로 배포되는** Java Web 이라는 코드베이스가 모든 사람이 하나의 코드에서 작업 하는 **방식/의미인** 모놀리틱 서비스라는데 있습니다.
> The problem with this architecture was that the codebase for Java Web was monolithic in the sense that everbody was contributing to one codebase that got deployed on a weekly or bi-weekly basis.

- 여기에서 문제점은 **어떠한 문제가 발생하였을 때**, **원인을 규명하기가 어렵다**는 점입니다. 우리는 아마도 천천히 발생하는 메모리 누수 문제 해결에 **일주일 넘게** 매달렸을 겁니다. 
> The problem with that was when a change was introduced that caused the problem, it was difficult to diagnose. We probably spent well over a week troubleshooting slow-moving memory leak. 

- **하루 정도 지나** 문제가 발생하면, 몇군데 코드를 **뽑아내어/제거한뒤** 다시 돌려보고 어떤 일이 발생하는지 지켜봅니다. 
> It took about a day to happen, we tried to pulling out pieces of code, and running it again to see what would happen.

- 하나의 어플리케이션에 대해 매우 많은 변화들이 한꺼번에 **이루어졌기/적용되었기 때문에**, 앞선 과정이 **장기간에 걸쳐** 진행되었습니다.
> Because so many changes were rolling into that one application, this took an extended period of time.

- 데이터베이스 또한 모놀리틱 이었으며 **더 심각한 상황**이었습니다. 그것은 스토어 데이터베이스라고 부르는 **한대의 하드웨어**에서 돌아가는 큰 하나의 오라클 데이터베이스였는데, 만약 **이것이 다운되면** 모든 서비스가 다운되었습니다.  
> The database was also monolithic in even a more severe sense. It was one piece of hardware, running one big Oracle database that we called the store database. And when this went down, everything went down. 

- 그리고 매년 명절 기간 **서비스 사용이 최고 점에 도달하기 시작하면**, 우리는 이 어플리케이션의 **수직적인 확장**을 위해 더 좋은 하드웨어를 **찾느라 허둥대고 있었습니다.**
> And every year as we started to get into the holiday peak, we were scrambling to find bigger and bigger hardware so that we could vertically scale this application.

- **아마도 발생했을** 서비스 **정지/불가상태** 외에 **공학적인 관점**에서 가장 힘들었던 부분은 바로 모든것이 **내부적으로 너무도 깊이 연결**되어 있었기에 **빠르게 움직일 수 없었던** 것이었습니다. 
> Probably one of the most painful pieces from the engineering perspective other than that the outages that might have happend was the lack of agility that we had because everything was so deeply interconnected.

- 우리는 직접 **데이터베이스를 호출**하고 있었고, 많은 어플리케이션들이 직접 테이블의 스키마를 **참조**하고 있었으며, 테이블에 칼럼을 하나 추가하는것이 **엄청 다양한 기능에 걸쳐진** 프로젝트 였던것을 기억하고 있습니다. 
> We had direct calls into the database, we had many applications directly referencing table schemas and I can remember trying to add a column to a table was a big cross-functional project for us. 

- 이런 방식은 **90년대 후반이나 2000년대 초반** 일반 적이었던 개발 패턴이었음에도 **불구하고** 현재는 **어떤 방식으로 서비스를 만들면 안되는지를** 잘 보여주는 훌륭한 예제 입니다. 
> This is a great example of how not to build services today although this was the common pattern back in the late 90s and early 2000s

--

- 이는 **다소** 추상적인 정의 입니다. **기술적으론** 굉장히 정확하지요. 하지만 제가 생각하는 마이크로 서비스를 **만든다는것이 어떤 의미인지**에 대해서는 **충분히 전달해 주지 못하는**것 같습니다. 
> It's a **somewhat** abstract definition. It's very technically correct. But it doesn't really **give you enough of a flavor** I think of **what it means to build** micro services

- 제가 마이크로 서비스에 대해서 **생각할 때**, 그건 제가 **과거 2000년도에** 경험했던 모놀리틱 서비스에 대한 극단적인 반작용 **으로 생각합니다.**
> **When I think about it**, I think of it as this extream reaction to that **experience that I had back in 2000** with monolithic applications. 

- 만약 당신이 이를 올바르게 사용한다면 그것들은 스스로 횡적으로 확장하기 쉽게 만듭니다. 
> They **tend to lend themselves to** horizontal scaling **if you approach it correctly**.

- 여러분은 하나의 작업을 좀 더 관리하기 쉬운 몇개의 작은 컴포넌트으로 쪼갤 수 있습니다. 
> You can take your work and **break it out into smaller components which make it more manageable.** 

- 여러분이 이러한 환경이 아니라면 마이크로 서비스를 운영하는것이 무척이나 더 힘들것입니다. 
> It's much much harder to manage micro services **if you are not doing it in** this kind of environment.

- 이제 넷플릭스의 구조를 간단히 보면서 어떻게 대응되는지 살펴봅시다. 
> Let's **take a look at the** Netflix architecture a little bit and **see how that maps**

- 저기 있는것은 저희 Neflix API 인데 API 게이트웨이로써 오늘날 우리의 시스템 구조에서 굉장히 중요한 부분을 차지하고 있으며 다른 모든 서비스들을 호출하여 고객의 요청을 수행하는 서비스 입니다.  
> There's our Neflix API which is our API gateway that today it's a **part of really core to our modern architecture** **calling out into all of the other services** to **fullfill requests** for customers. 

- 저기에는 몇개의 부가 서비스들도 있습니다.
> There's some **a few auxiliary services** as well.

