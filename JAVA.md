# Java/process conventions

**Backend development conventions general (approach)**

1. We are trying to validate our assumptions and understand the entire flow before writing any code (although it’s fine to quickly write some quick PoC code to validate assumptions if needed)
2. Naming consistency is very important.
3. One class has to do one things and code should be readable
4. When coding we use keyboard and shortcuts as much as possible, trying to avoid the mouth.
5. We respect Intellij idea highlights and fix them
6. When we start our work day we do pull from repo
7. If somewhere in the method we have a variable that we are going to return as a result of method execution we should call this variable `result` (of course if needs to be stored as variable - there’s no need to create an artificial variable just to follow the convention) I took this convention from Martin Fowler and it’s wonderful - each time you see a `result` variable you know that it’s the result of method execution.

**Security**

1. When user performs any sensitive operation service layer checks if he has the right to do so

**General (technical)**

1. For all money-related operations we use BigDecimal.

**Lombok**

1. @Slf4j instead of manual declaration of logger class
2. @AllArgsConstructor/@RequiredArgsConstructor instead of manually declaring constructor

**REST**

1. REST: POST -> add, PUT -> modify, GET -> query, DELETE -> remove, consistency with ‘s’ on entities
2. All incoming dto fields are having validations and dto’s are marked with @Valid
3. If there is a dto that has success flag, there are two factory methods: success/error and dto takes care of creating itself
4. If we have a controller that accepts dto as a request and produces dto as a response, than request dto should be named: `RequestDto` , response dto: `ResponseDto` and service method that is handling the request should have a method `ResponseDto handle(*RequestDto request)` .