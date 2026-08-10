# Java/Agentic AI Technical Interview Guide
## For: Senior Java Full Stack Developer (11+ years)

**Candidate:** Nagaswetha Vempati  
**Target Role:** Java/Agentic AI  
**Interview Purpose:** Verify depth of AI/LLM integration experience, core Java mastery, and design pattern fluency. Separate genuine expertise from resume padding.

---

## TABLE OF CONTENTS

**SECTION 1: AI/AGENTIC WORKFLOWS VERIFICATION**
- [1.1 LangChain & Agent Architecture](#11-langchain--agent-architecture)
- [1.2 LLM Integration Depth](#12-llm-integration-depth)
- [1.3 Prompt Engineering & Guardrails](#13-prompt-engineering--guardrails)
- [1.4 Vector Search & Embeddings](#14-vector-search--embeddings)

**SECTION 2: CORE JAVA FUNDAMENTALS**
- [2.1 Collections & Generics](#21-collections--generics)
- [2.2 Exception Handling](#22-exception-handling)
- [2.3 Streams API & Functional Programming](#23-streams-api--functional-programming)
- [2.4 Memory & GC](#24-memory--gc)

**SECTION 3: OBJECT-ORIENTED DESIGN**
- [3.1 OOP Pillars](#31-oop-pillars)
- [3.2 Encapsulation & Immutability](#32-encapsulation--immutability)
- [3.3 Inheritance vs Composition](#33-inheritance-vs-composition)

**SECTION 4: SOLID PRINCIPLES**
- [4.1 Single Responsibility](#41-single-responsibility)
- [4.2 Open/Closed](#42-openclosed)
- [4.3 Liskov Substitution](#43-liskov-substitution)
- [4.4 Interface Segregation](#44-interface-segregation)
- [4.5 Dependency Inversion](#45-dependency-inversion)

**SECTION 5: DESIGN PATTERNS**
- [5.1 Creational Patterns](#51-creational-patterns)
- [5.2 Structural Patterns](#52-structural-patterns)
- [5.3 Behavioral Patterns](#53-behavioral-patterns)
- [5.4 Microservices Patterns](#54-microservices-patterns)

**SECTION 6: CONCURRENCY & ASYNC**
- [6.1 Multithreading Fundamentals](#61-multithreading-fundamentals)
- [6.2 CompletableFuture](#62-completablefuture)
- [6.3 Virtual Threads & Structured Concurrency](#63-virtual-threads--structured-concurrency)
- [6.4 Concurrency Pitfalls](#64-concurrency-pitfalls)

**SECTION 7: SPRING BOOT & MICROSERVICES**
- [7.1 Spring Architecture](#71-spring-architecture)
- [7.2 Microservices Design](#72-microservices-design)
- [7.3 Event-Driven Architecture](#73-event-driven-architecture)

**SECTION 8: FOLLOW-UP & PROBE QUESTIONS**
- [8.1 Red Flag Patterns](#81-red-flag-patterns)
- [8.2 Deep Dive Templates](#82-deep-dive-templates)
- [8.3 Real-Time Analysis Guide](#83-real-time-analysis-guide)

---

# SECTION 1: AI/AGENTIC WORKFLOWS VERIFICATION

## 1.1 LangChain & Agent Architecture

**QUESTION 1.1.1 (Foundational)**
"In the LangChain agentic workflows you built at KeyBank, walk me through the component architecture. Specifically, what are the roles of the Agent, the Tools, the LLM, and the Memory layer in one concrete example you shipped?"

*What you're listening for:*
- Do they distinguish between AgentExecutor and LLMChain?
- Can they name specific tool definitions and how tools are called?
- Do they discuss ReAct (Reasoning + Acting) loop?
- Vague answer = resume padding. Specific answer = real work.

**QUESTION 1.1.2 (Architecture)**
"Tell me about a situation where your agent needed to call multiple tools in sequence. What did you use for orchestration, and how did you handle tool failures or hallucinations where the agent called a tool that didn't exist?"

*What you're listening for:*
- Do they mention max_iterations, early stopping?
- Have they dealt with error handling in tool chains?
- Can they articulate what happens when an agent gets stuck?
- Do they know the difference between Serial and Parallel tool execution?

**QUESTION 1.1.3 (Production Risk)**
"You mention implementing 'multi-step reasoning' in agentic workflows. Walk me through a case where you had to add validation or guardrails because the agent was doing something unsafe or incorrect. What went wrong first, and how did you detect it?"

*What you're listening for:*
- Real production incidents demonstrate depth
- Specific guardrails: output validation, tool access control, PII masking
- Testing strategy for agents (hard problem)
- Monitoring/observability for agent behavior

**QUESTION 1.1.4 (Trade-offs)**
"When would you NOT use an agentic approach and just call the LLM directly or use a fixed prompt? Give me an example from your work."

*What you're listening for:*
- Do they understand cost and latency trade-offs?
- Can they articulate when determinism matters?
- Maturity signal: knowing the limits of your own approach

---

## 1.2 LLM Integration Depth

**QUESTION 1.2.1 (Model Awareness)**
"You've worked with OpenAI, Titan, Vertex AI, and Claude according to your resume. Tell me a transaction or workflow where you chose one model over another. What were the selection criteria, and what tradeoffs did you make?"

*What you're listening for:*
- Do they know model-specific strengths (e.g., Claude for long context, GPT-4 for code)?
- Can they articulate cost per token or latency differences?
- Do they discuss context window limits and how to work around them?
- Vague: "they're all the same" = red flag

**QUESTION 1.2.2 (API Integration)**
"Describe how you integrated an LLM API into a Spring Boot microservice. How did you handle rate limiting, retries, timeouts, and cost tracking? What went wrong in production?"

*What you're listening for:*
- Specific Spring Boot patterns (RestTemplate, WebClient, Feign)?
- Resilience4j circuit breaker + retry logic?
- Did they monitor token usage and costs?
- Cost explosion incidents (common mistake)

**QUESTION 1.2.3 (Latency & Streaming)**
"LLM responses can take 5-30 seconds. How did you handle this in a banking UI where users expect sub-second responses? Did you stream responses? How?"

*What you're listening for:*
- Do they understand streaming vs. waiting?
- Did they use Server-Sent Events (SSE) in React?
- BackendÅ: How did they stream from LLM API to client?
- Loading UX patterns for long-running LLM calls?

**QUESTION 1.2.4 (Context & Token Management)**
"You mention semantic search and embeddings. How did you decide what data to embed, what vector DB to use, and how you prevent hallucinations by grounding LLM responses in retrieved documents?"

*What you're listening for:*
- Chunking strategy (sentence, paragraph, semantic)?
- Embedding model choice (OpenAI, Cohere, local)?
- Retrieval strategy (similarity search, BM25 hybrid)?
- How they handle stale or outdated embeddings?
- Reranking before sending to LLM?

---

## 1.3 Prompt Engineering & Guardrails

**QUESTION 1.3.1 (Prompt Structure)**
"Show me your approach to a complex prompt for banking. How do you structure it? Do you use examples, role definitions, constraints? Walk me through one real example."

*What you're listening for:*
- Do they use few-shot prompting with examples?
- Role definition: "You are a financial advisor"?
- Explicit constraints: "Do not disclose customer PII"?
- Do they test prompt variations (A/B testing)?
- Do they version control prompts?

**QUESTION 1.3.2 (Security & Compliance)**
"The resume mentions PII masking and data filtering. In a banking LLM application, walk me through how you ensured customer data wasn't leaked via the LLM prompt or response. What specific techniques did you use?"

*What you're listening for:*
- Pre-processing: regex/NLP-based PII detection before sending to LLM?
- Tokenization strategies to prevent leakage?
- Post-processing: output validation for leaked data?
- Did they use a dedicated PII detection library?
- Audit logging of what went in/out of LLM?

**QUESTION 1.3.3 (Output Validation)**
"LLMs hallucinate. In your banking fraud detection use case, how did you validate the LLM's suggestions before acting on them? What if the LLM returned a transaction that didn't exist?"

*What you're listening for:*
- Validation logic: schema checking, fact checking?
- Confidence scoring?
- Human-in-loop workflows?
- Fallback strategies when validation fails?

**QUESTION 1.3.4 (Evaluation & Metrics)**
"How do you measure if an agentic workflow is actually working correctly? What metrics did you track? Did you have a testing strategy?"

*What you're listening for:*
- Metrics: accuracy, latency, cost, hallucination rate?
- A/B testing LLM strategies?
- Golden test set against expected outputs?
- Red flag: no metrics = no real production experience

---

## 1.4 Vector Search & Embeddings

**QUESTION 1.4.1 (Database Choice)**
"You mention PgVector and Pinecone in your experience. Why did you choose one over the other in specific situations? What are the trade-offs?"

*What you're listening for:*
- PgVector: embedded, scales to millions
- Pinecone: managed, scales to billions, more expensive
- Latency vs. cost trade-offs?
- Integration with existing DB vs. separate service?

**QUESTION 1.4.2 (Semantic Search Implementation)**
"Walk me through how you built semantic search for financial documents. From ingestion to query to ranking, what's the full pipeline?"

*What you're listening for:*
- Chunking strategy (financial docs can be long)
- Embedding model choice and cost implications
- Query expansion (did they improve search via query rewriting)?
- Ranking/reranking before sending to LLM?
- Metadata filtering (date ranges, document type)?

**QUESTION 1.4.3 (Embedding Staleness)**
"If a document is updated, how do you ensure the vector embeddings stay in sync? What's your strategy for re-embedding?"

*What you're listening for:*
- Do they understand this is a real problem?
- Versioning strategy?
- Incremental updates or full re-embedding?
- How they detect what needs updating?

---

# SECTION 2: CORE JAVA FUNDAMENTALS

## 2.1 Collections & Generics

**QUESTION 2.1.1 (Collections Depth)**
"Explain the difference between ArrayList and LinkedList. Not just the theoretical difference, but when would you actually use each in production, and what were the performance implications in code you've written?"

*What you're listening for:*
- ArrayList: O(1) access, O(n) insertion/deletion (except end)
- LinkedList: O(n) access, O(1) insertion/deletion
- Real scenario: "We used LinkedList for a queue where we knew we'd remove from front" = depth
- Do they mention Cache locality for ArrayList?

**QUESTION 2.1.2 (Generics & Type Erasure)**
"You've worked with Java Generics extensively. Explain type erasure. Why does `List<Integer> list = new ArrayList<Long>()` not work at compile time, but you can't check `if (obj instanceof List<String>)` at runtime?"

*What you're listening for:*
- Understanding of compile-time vs. runtime
- Can they explain why?
- Do they know workarounds (bounded wildcards, reifiable types)?
- Can they articulate the JVM limitations?

**QUESTION 2.1.3 (HashMap vs. ConcurrentHashMap)**
"When would you use ConcurrentHashMap over HashMap in a multithreaded environment? Is thread-safe always better?"

*What you're listening for:*
- HashMap not thread-safe
- ConcurrentHashMap uses segment locking (Java 7) or node-level locking (Java 8+)
- Trade-off: memory overhead for concurrency
- Knowledge of when synchronization isn't worth it (single-threaded use within synchronized block is fine)

**QUESTION 2.1.4 (Collections Hierarchy)**
"Draw or describe the Collections hierarchy. Where do Queue, Deque, Set fit? Why is Vector legacy and what replaced it?"

*What you're listening for:*
- Proper hierarchy understanding
- Vector -> ArrayList (legacy synchronized)
- Map hierarchy (HashMap, TreeMap, ConcurrentHashMap)
- Immutable collections and when to use them

---

## 2.2 Exception Handling

**QUESTION 2.2.1 (Checked vs. Unchecked)**
"Explain the difference between checked and unchecked exceptions. In your microservices, how do you decide which to use? Show me an example."

*What you're listening for:*
- Checked: must catch or declare
- Unchecked: RuntimeException and subclasses
- Modern Java practice: prefer unchecked in most cases
- But: checked for recoverable conditions (IOException)?
- Do they understand the debate (Spring doesn't force checked)?

**QUESTION 2.2.2 (Exception Hierarchy)**
"Design an exception hierarchy for a payment processing system. How would you structure custom exceptions? What information would you include?"

*What you're listening for:*
- Root cause exception
- Hierarchical organization (PaymentException -> InvalidAmountException)
- Include context (transaction ID, timestamp, amount)?
- Stacktrace preservation?
- Do they use exception cause chaining?

**QUESTION 2.2.3 (Try-Catch-Finally vs. Try-With-Resources)**
"In a database connection scenario, what's the difference between try-finally and try-with-resources? Why is one better?"

*What you're listening for:*
- Try-with-resources handles suppression properly
- Automatic resource closing
- Multiple exceptions and addSuppressed()
- Can they show this in Spring/Hibernate context?

**QUESTION 2.2.4 (Exception Handling in Async Code)**
"In CompletableFuture chains, how do you handle exceptions from async operations? Show me how you'd catch and handle an exception in a chain."

*What you're listening for:*
- exceptionally() method
- handle() method with Throwable
- Difference between them
- Do they know exception doesn't propagate through future chains automatically?

---

## 2.3 Streams API & Functional Programming

**QUESTION 2.3.1 (Streams Fundamentals)**
"Explain what a Stream is. Is it a collection? Can you iterate a stream twice? Show me an example from your code where you used a stream."

*What you're listening for:*
- Stream is NOT a collection, lazy evaluation
- Cannot iterate twice (terminal operation consumes it)
- Intermediate vs. terminal operations
- Specific example: filtering, mapping, collecting?

**QUESTION 2.3.2 (Intermediate vs. Terminal Operations)**
"Give me an example of an intermediate operation and a terminal operation. What's the difference? Why does it matter?"

*What you're listening for:*
- Intermediate: filter, map, flatMap, distinct (return Stream)
- Terminal: forEach, collect, reduce, findFirst (void or value)
- Lazy evaluation: intermediateoperation only evaluated when terminal called
- Performance implication: can short-circuit (findFirst stops at first match)

**QUESTION 2.3.3 (Functional Interfaces)**
"What's a functional interface? Create one. Show me how you'd use it in a lambda expression. Have you created custom functional interfaces in your Spring code?"

*What you're listening for:*
- Single abstract method
- @FunctionalInterface annotation (not required but good practice)
- Common ones: Predicate, Function, Consumer, Supplier
- Real example: Spring's ApplicationEventPublisher, Callback patterns?

**QUESTION 2.3.4 (Parallel Streams & Performance)**
"You've used Streams API. When would you use parallel streams? What's the overhead? Show me a case where parallel hurt performance."

*What you're listening for:*
- Parallel useful for large datasets with expensive operations
- Overhead: thread pool, coordination
- Can hurt with small datasets or I/O-bound work
- ForkJoinPool, splitting strategy?
- Do they understand data locality?

**QUESTION 2.3.5 (Reduce & Collect)**
"Explain the difference between reduce() and collect(). When would you use each? Show me examples."

*What you're listening for:*
- reduce(): combines stream into single value (mathematical operations)
- collect(): groups into collection (practical data reshaping)
- reduce() on empty stream (optional)
- Collectors.toList(), groupingBy(), partitioningBy()?

---

## 2.4 Memory & GC

**QUESTION 2.4.1 (Heap vs. Stack)**
"Explain heap vs. stack memory. Where do objects live? Where do primitives live? What happens in each when you go out of scope?"

*What you're listening for:*
- Stack: method calls, local variables, primitives, object references
- Heap: object instances
- Stack automatically freed when method exits
- Heap managed by GC
- References on stack point to objects on heap

**QUESTION 2.4.2 (Garbage Collection)**
"Explain how garbage collection works in Java. You've worked with high-throughput systems. Have you tuned GC? What GC algorithms do you know?"

*What you're listening for:*
- Mark-and-sweep basics
- Generational GC (young/old generation)
- Stop-the-world pauses
- GC algorithms: G1GC (default), ZGC (low-latency), Shenandoah
- In banking: latency-sensitive, might use ZGC?
- Monitoring GC with jstat, JVisualVM?

**QUESTION 2.4.3 (Memory Leaks)**
"In your microservices, have you encountered memory leaks? How did you detect them? How do you prevent them in Java?"

*What you're listening for:*
- Memory leak: unreferenced objects can't be GC'd (usually holding references)
- Common causes: static collections, listeners not unregistered, thread local not cleaned
- Detection: heap dump analysis, jhat, Eclipse MAT
- Prevention: weak references, nulling references, proper lifecycle management

**QUESTION 2.4.4 (Weak/Soft/Phantom References)**
"When would you use a WeakReference, SoftReference, or PhantomReference? Give me a real scenario."

*What you're listening for:*
- WeakReference: GC eligible even if referenced (caches)
- SoftReference: GC only under memory pressure (better caches)
- PhantomReference: cleanup notifications before object freed
- Example: Spring's WeakHashMap for caching request-scoped objects

---

# SECTION 3: OBJECT-ORIENTED DESIGN

## 3.1 OOP Pillars

**QUESTION 3.1.1 (Encapsulation)**
"Define encapsulation. Show me a bad example and a good example of encapsulation in Java. How does this relate to SOLID?"

*What you're listening for:*
- Hiding implementation, exposing interface
- Private fields, public getters/setters (controlled access)
- Bad: public fields that can be modified arbitrarily
- Good: validation in setters, immutable fields when possible
- Encapsulation -> S (Single Responsibility), D (Dependency Inversion)

**QUESTION 3.1.2 (Inheritance)**
"You've been in Java since early days. How has inheritance changed in Java? When should you use inheritance vs. composition?"

*What you're listening for:*
- Inheritance creates tight coupling
- "Favor composition over inheritance" (GoF)
- Liskov Substitution Principle governs proper inheritance
- Example: Animal -> Dog is proper, but not always
- Interface vs. abstract class (Java 8+ interface methods changed this)

**QUESTION 3.1.3 (Polymorphism)**
"Explain method overriding and method overloading. When you call a method on an object, how does the JVM decide which one to call?"

*What you're listening for:*
- Overloading: compile-time (static dispatch)
- Overriding: runtime (dynamic dispatch)
- Virtual method table lookup
- Can you override final methods? (No)
- Can a subclass broaden or narrow the return type? (Covariance allowed, contravariance for parameters not allowed)

**QUESTION 3.1.4 (Abstraction)**
"Design an abstract class for a payment processor. What methods would be abstract? What concrete? How would subclasses use this?"

*What you're listening for:*
- Abstract methods: contract subclasses must honor
- Concrete methods: shared implementation
- Abstract class vs. interface (interface is contract, abstract class is contract + some implementation)
- Modern Java: interface with default methods blurs this line

---

## 3.2 Encapsulation & Immutability

**QUESTION 3.2.1 (Mutable vs. Immutable Objects)**
"Create an immutable class for a Transaction object. What do you have to do? Why is this hard with inheritance?"

*What you're listening for:*
- Final class, final fields
- Private constructor or factory
- No setters
- Defensive copies in getters if returning mutable objects
- Immutable: String, Integer, LocalDate (good examples)

**QUESTION 3.2.2 (Defensive Copying)**
"You return a List from a getter. Why is `return list;` bad? How do you fix it?"

*What you're listening for:*
- Client can modify internal list
- Fix: `return Collections.unmodifiableList(list)` or `return new ArrayList<>(list)`
- Trade-off: performance vs. safety
- In banking: immutability is often worth the cost

**QUESTION 3.2.3 (JavaBeans vs. Records)**
"With Java Records (17+), how do you create immutable data classes now? Are you still using JavaBeans getters/setters?"

*What you're listening for:*
- Records are immutable by default
- No getters/setters needed (just use field names)
- Have they used Records in recent projects?
- Canonical constructor, compact constructor?

---

## 3.3 Inheritance vs. Composition

**QUESTION 3.3.1 (Inheritance Problem)**
"Design a hierarchy: Animal, Dog, Cat. Now someone asks you to add a class for Penguin. You realize penguins don't bark. Your inheritance breaks down. How do you fix it? What would you have done differently?"

*What you're listening for:*
- Animal -> Dog, Cat: both bark? (Wrong assumption)
- Better: separate interface Barks from Animal
- Or: composition (Dog has-a Vocalizer)
- Do they see the LSP violation?

**QUESTION 3.3.2 (Composition Example)**
"Give me a real example from your codebase where you used composition instead of inheritance. Why was it better?"

*What you're listening for:*
- Specific example required
- Flexibility: can swap implementations
- Reduced coupling
- Example: Strategy pattern (not inheriting behavior, composing it)

**QUESTION 3.3.3 (Multiple Inheritance & Java)**
"Java doesn't allow multiple inheritance. Why? How do interfaces solve this? What's the diamond problem?"

*What you're listening for:*
- Diamond: two parents, one grandparent (which version?)
- Interfaces: contract only, not implementation (pre-Java 8)
- Java 8+ default methods make interfaces more like abstract classes
- Do they understand the risk of default methods?

---

# SECTION 4: SOLID PRINCIPLES

## 4.1 Single Responsibility

**QUESTION 4.1.1 (Definition & Example)**
"Define Single Responsibility Principle. Show me a class that violates it and refactor it to follow SRP."

*What you're listening for:*
- One reason to change
- Example: UserService that does auth, validation, persistence (should split)
- After refactor: AuthService, UserValidator, UserRepository
- Do they recognize SRP violations in their code?

**QUESTION 4.1.2 (Banking Context)**
"In your payment processing system, you have a PaymentProcessor class. What responsibilities should it NOT have? What separate classes would you create?"

*What you're listening for:*
- PaymentProcessor: calculate, authorize, process
- Separate: PII masking, audit logging, fraud detection, settlement
- Each can change independently
- Team can own different responsibilities

---

## 4.2 Open/Closed

**QUESTION 4.2.1 (Definition)**
"Define Open/Closed Principle. Show me a violation and fix it using polymorphism."

*What you're listening for:*
- Open for extension, closed for modification
- Violation: if-else chains checking type (if this payment type, do X; if that payment type, do Y)
- Fix: interface PaymentProcessor, different implementations (ACHProcessor, SEPAProcessor)
- Add new payment type without changing existing code

**QUESTION 4.2.2 (Real Scenario)**
"In your banking app, you support ACH, SEPA, and SWIFT. Tomorrow, CBDC payments are required. How do you extend without modifying existing code?"

*What you're listening for:*
- New CBDCProcessor implements PaymentProcessor
- Existing ACH/SEPA unchanged
- Registry pattern to discover processors?
- Configuration: add new processor to config, not code changes

---

## 4.3 Liskov Substitution

**QUESTION 4.3.1 (Definition & Violation)**
"Define Liskov Substitution Principle. Show me a violation. A classic example: Square extends Rectangle."

*What you're listening for:*
- Subtype must be substitutable for parent type
- Square violates Rectangle contract: if Rectangle.setWidth(), height changes too (Square.setWidth() changes height)
- Square is NOT substitutable for Rectangle
- Violation: code expecting Rectangle breaks with Square
- Fix: don't inherit (composition instead)

**QUESTION 4.3.2 (Behavioral Subtyping)**
"In your payment processors, what could violate LSP? For example, if all PaymentProcessors should retry on failure, but one doesn't. How do you enforce this?"

*What you're listening for:*
- Contract must hold for all implementations
- Code calling PaymentProcessor assumes retry behavior
- Violation: one processor throws and doesn't retry
- Solution: enforce in interface (retry in abstract base or adapter)
- Testing: test all implementations the same way

---

## 4.4 Interface Segregation

**QUESTION 4.4.1 (Definition)**
"Define Interface Segregation Principle. Show me a bloated interface and split it into focused ones."

*What you're listening for:*
- Clients shouldn't depend on methods they don't use
- Bad: UserService with getUser(), saveUser(), deleteUser(), sendEmail(), createReport()
- Split: UserRepository (get, save, delete), EmailService (send), ReportService (create)
- Interfaces: IUserRepository, IEmailSender, IReportGenerator

**QUESTION 4.4.2 (Spring Context)**
"You've built Spring services. Is a monolithic service interface better or split into focused interfaces? Give me an example."

*What you're listening for:*
- Split: easier to mock, easier to understand, easier to extend
- Example: OrderService split into OrderCreation, OrderRetrieval, OrderCancellation
- Clients use only what they need
- Mockito easier to mock focused interfaces

---

## 4.5 Dependency Inversion

**QUESTION 4.5.1 (Definition)**
"Define Dependency Inversion Principle. Show me code with tight coupling and refactor it using DI."

*What you're listening for:*
- High-level modules shouldn't depend on low-level modules; both depend on abstractions
- Violation: PaymentService creates new ACHProcessor() directly
- Fix: PaymentService receives PaymentProcessor (interface) via constructor
- Spring @Autowired: DI framework handles this

**QUESTION 4.5.2 (Spring & DI)**
"How does Spring's dependency injection work? What's the difference between @Autowired, constructor injection, and setter injection? Which do you prefer and why?"

*What you're listening for:*
- @Autowired: field injection (convenient but can create circular dependencies, hard to test)
- Constructor injection: enforces dependencies, good for testing, hard with optional dependencies
- Setter injection: optional dependencies
- Best practice: constructor injection (dependencies explicit, immutable, testable)
- Do they prefer one and can articulate why?

**QUESTION 4.5.3 (Inversion of Control)**
"What's Inversion of Control? How is it different from Dependency Injection?"

*What you're listening for:*
- IoC: framework controls object lifecycle (vs. manual creation)
- DI: framework injects dependencies
- DI is one form of IoC
- IoC: Spring manages beans, wiring, initialization order, etc.

---

# SECTION 5: DESIGN PATTERNS

## 5.1 Creational Patterns

**QUESTION 5.1.1 (Singleton)**
"Explain the Singleton pattern. Create a thread-safe Singleton in Java. What are the pitfalls?"

*What you're listening for:*
- One instance, global access
- Bill Pugh (static nested class) or Enum (best in Java)
- Double-checked locking (not needed with Bill Pugh)
- Pitfalls: serialization, reflection can break singleton
- Enum singleton: thread-safe, serialization-safe, reflection-safe

**QUESTION 5.1.2 (Builder Pattern)**
"Show me a Builder pattern implementation for creating complex objects. Why is this better than telescoping constructors?"

*What you're listening for:*
- Fluent API: new TransactionBuilder().amount(100).currency("USD").build()
- Telescoping constructors: Transaction(amount), Transaction(amount, currency), etc. (ugly)
- Optional fields easily handled in Builder
- Immutable objects after build
- In payment systems: Building complex Transaction objects?

**QUESTION 5.1.3 (Factory Pattern)**
"Explain Factory pattern. Show me how you'd use it for creating different PaymentProcessor implementations."

*What you're listening for:*
- Abstract object creation
- PaymentProcessorFactory.createProcessor("ACH") returns ACHProcessor
- Decouples client from concrete class
- Configuration-driven (which processor based on type)?

**QUESTION 5.1.4 (Abstract Factory)**
"What's the difference between Factory and Abstract Factory? Give an example."

*What you're listening for:*
- Factory: creates one type of object
- Abstract Factory: creates families of related objects
- Example: UIFactory -> Button, Checkbox, Dialog (whole family) vs. ButtonFactory -> just Button
- Less common, but useful for multi-theme systems

---

## 5.2 Structural Patterns

**QUESTION 5.2.1 (Adapter Pattern)**
"Explain the Adapter pattern. Give me a real scenario where you used it. How does it differ from Decorator?"

*What you're listening for:*
- Adapter: translates interface (old code to new interface)
- Example: integrating legacy SOAP service into modern REST API (SOAPAdapter)
- Decorator: adds behavior without changing interface
- Both wrap objects but with different purposes

**QUESTION 5.2.2 (Decorator Pattern)**
"Show me a Decorator pattern for adding features to a PaymentProcessor (logging, caching, validation). Why is this better than inheritance?"

*What you're listening for:*
- Decorator: wraps component, adds behavior
- Example: LoggingPaymentProcessor(realProcessor), CachingPaymentProcessor(realProcessor)
- Chain: LoggingPaymentProcessor(CachingPaymentProcessor(realProcessor))
- Flexibility: add/remove behavior at runtime
- Inheritance: can't combine features easily, rigid

**QUESTION 5.2.3 (Proxy Pattern)**
"Explain Proxy pattern. How is it different from Decorator? Give me a scenario from microservices."

*What you're listening for:*
- Proxy: controls access to real object
- Lazy loading: Proxy creates real object only when needed
- Remote proxy: JpaRepository proxy for database (Spring Data JPA)
- Caching proxy: Proxy caches results
- Access control proxy: Proxy checks permissions before allowing access

**QUESTION 5.2.4 (Facade Pattern)**
"Explain Facade pattern. In your microservices, have you used it? What problem does it solve?"

*What you're listening for:*
- Facade: simplifies complex subsystem
- Example: PaymentFacade hides complexity of ACH, SEPA, SWIFT, Fraud Detection, Audit
- Client calls paymentFacade.processPayment(), doesn't care about complexity
- Common in service layers (Spring services are often facades)

---

## 5.3 Behavioral Patterns

**QUESTION 5.3.1 (Strategy Pattern)**
"Explain Strategy pattern. Design a sorting system where you can switch algorithms (merge sort, quick sort) at runtime."

*What you're listening for:*
- Strategy: family of algorithms, encapsulate each
- Interface SortStrategy, implementations MergeSortStrategy, QuickSortStrategy
- Context: Sorter uses strategy
- Runtime switch: new Sorter(new QuickSortStrategy())
- In banking: FraudStrategy, PricingStrategy?

**QUESTION 5.3.2 (Observer Pattern)**
"Explain Observer pattern. Spring uses this heavily. Show me how you'd implement an event system in your microservices."

*What you're listening for:*
- Observer: subjects notify observers of state changes
- Spring: ApplicationEventPublisher / @EventListener
- Or: manually implementing Observer interface
- Use case: payment completed, notify ledger, audit, notification service
- Async: @Async @EventListener, or Spring Kafka events

**QUESTION 5.3.3 (State Pattern)**
"Explain State pattern. Model a payment's lifecycle: New -> Authorized -> Settled -> Completed. Each state has different behaviors."

*What you're listening for:*
- State: encapsulate state-dependent behavior
- PaymentState interface, implementations NewState, AuthorizedState, etc.
- Payment delegates to state
- State can transition: payment.settle() -> new state transitions
- Enum-based states simpler for small state machines?

**QUESTION 5.3.4 (Chain of Responsibility)**
"Explain Chain of Responsibility. Design a validation chain for payments (fraud check, KYC check, amount limit check)."

*What you're listening for:*
- Chain: handlers in sequence, each can handle or pass to next
- Validator interface, implementations FraudValidator, KYCValidator, LimitValidator
- Each validator checks condition, passes to next if satisfied
- Stops at first failure
- vs. if-else chains: more maintainable

**QUESTION 5.3.5 (Template Method)**
"Explain Template Method pattern. Design an abstract payment processor where subclasses implement specific steps."

*What you're listening for:*
- Template Method: abstract method defines algorithm steps (template), subclasses fill in details
- AbstractPaymentProcessor: process() calls validate(), authorize(), settle(), notify()
- Subclasses implement each step
- HollywoodPrinciple: don't call us, we'll call you (framework calls subclass methods)

---

## 5.4 Microservices Patterns

**QUESTION 5.4.1 (Circuit Breaker)**
"Explain Circuit Breaker pattern. You mention using Resilience4j. Show me how you'd implement circuit breaker for calling an external payment gateway."

*What you're listening for:*
- States: Closed (normal), Open (failing, reject calls), Half-Open (testing recovery)
- Threshold: N failures -> Open
- Timeout: stay Open for T seconds -> Half-Open
- Success in Half-Open -> Closed
- Prevents cascading failures
- Resilience4j: @CircuitBreaker annotation

**QUESTION 5.4.2 (Bulkhead Pattern)**
"Explain Bulkhead pattern. How would you isolate payment processor failures from notification service failures?"

*What you're listening for:*
- Bulkhead: separate resource pools for different operations
- Thread pools: payment thread pool, notification thread pool
- Failure in one doesn't starve the other
- Resilience4j: @Bulkhead annotation with pool size limit
- Prevents one slow service from taking down entire system

**QUESTION 5.4.3 (Retry Pattern)**
"When would you retry? When would you NOT? Show me retry logic you've implemented."

*What you're listening for:*
- Retry: idempotent operations (safe to retry)
- Don't retry: non-idempotent (money transfer) unless idempotency key tracked
- Backoff strategy: exponential backoff, jitter?
- Max retries: don't retry forever
- Resilience4j: @Retry annotation

**QUESTION 5.4.4 (Saga Pattern)**
"Explain Saga pattern for distributed transactions. In your banking system, if you need to debit account A and credit account B (two services), how do you ensure consistency?"

*What you're listening for:*
- Saga: long-running transaction split into local transactions
- Choreography: each service emits events, others react (reactive)
- Orchestration: central orchestrator coordinates steps (explicit)
- Compensation: if step fails, compensate (undo previous steps)
- Example: Payment saga: authorize() -> settle() -> notify(). If settle() fails, compensate authorize()

**QUESTION 5.4.5 (API Gateway Pattern)**
"Explain API Gateway pattern. In your microservices, do you use it? What does it do?"

*What you're listening for:*
- API Gateway: single entry point for clients
- Responsibilities: routing, rate limiting, authentication, logging, request/response transformation
- Tools: AWS API Gateway, Nginx, Spring Cloud Gateway
- Reduces client complexity (clients don't call multiple services)
- Centralized concerns: security, logging, rate limiting

---

# SECTION 6: CONCURRENCY & ASYNC

## 6.1 Multithreading Fundamentals

**QUESTION 6.1.1 (Thread Lifecycle)**
"Explain the thread lifecycle in Java. What are the states?"

*What you're listening for:*
- New: created, not started
- Runnable: started, waiting for CPU or running
- Blocked/Waiting: waiting for lock or IO
- Timed Waiting: waiting with timeout
- Terminated: finished

**QUESTION 6.1.2 (Synchronization)**
"Explain synchronized keyword. What does it actually do? Show me how you'd synchronize access to a shared counter."

*What you're listening for:*
- Monitor/intrinsic lock: every object has one
- synchronized method: lock on 'this'
- synchronized block: lock on specific object
- Visibility: happens-before relationships
- Example: synchronized int increment() { return ++counter; }
- Race condition: without sync, reads/writes can interleave

**QUESTION 6.1.3 (Volatile & Atomic)**
"When do you use volatile vs. synchronized vs. AtomicInteger? Give me scenarios."

*What you're listening for:*
- volatile: visibility across threads, but not atomicity
- synchronized: mutual exclusion
- AtomicInteger: atomic operations without locking (CAS - compare-and-swap)
- Volatile example: boolean shutdown flag
- Atomic example: counter increments without lock
- Don't overuse synchronized (performance)

**QUESTION 6.1.4 (Deadlock)**
"What's deadlock? Show me code that deadlocks. How do you prevent it?"

*What you're listening for:*
- Deadlock: threads waiting on each other, never progress
- Example: Thread1 locks A, waits for B; Thread2 locks B, waits for A
- Prevention: acquire locks in same order, use timeout (tryLock), avoid nested locks
- Detection: jstack, JVisualVM show deadlocks

**QUESTION 6.1.5 (Wait/Notify)**
"Explain wait() and notify(). Show me a producer-consumer pattern using wait/notify."

*What you're listening for:*
- wait(): release lock, wait for notification
- notify(): wake one waiting thread
- notifyAll(): wake all waiting threads
- Must be in synchronized block
- Producer puts item, notifies consumer
- Consumer waits for item, retrieves, notifies producer
- Modern alternative: BlockingQueue, Condition (ReentrantLock)

---

## 6.2 CompletableFuture

**QUESTION 6.2.1 (Fundamentals)**
"Explain CompletableFuture. What's the difference between Future and CompletableFuture?"

*What you're listening for:*
- Future: represents async computation result, blocking get()
- CompletableFuture: composable, can chain operations, can complete manually
- Create: CompletableFuture.supplyAsync(() -> fetchData())
- Chain: future.thenApply(data -> transform(data)).thenAccept(result -> log(result))

**QUESTION 6.2.2 (Chaining Operations)**
"Show me a chain of CompletableFuture operations. You call a payment gateway API, transform response, save to DB, send notification. All async."

*What you're listening for:*
- supplyAsync(): start with computation
- thenApply(): transform result
- thenCompose(): chain async operations (flatMap)
- thenAccept(): consume result (side effect)
- Error handling: exceptionally(), handle()
- Composition: multiple futures combined with allOf(), anyOf()

**QUESTION 6.2.3 (Error Handling)**
"In your payment processing chain, the gateway returns 503 (service unavailable). How do you handle this in CompletableFuture?"

*What you're listening for:*
- exceptionally(ex -> fallbackValue)
- handle((result, ex) -> result != null ? result : fallbackValue)
- retry on specific exceptions
- Timeouts: orTimeout(), completeOnTimeout()
- Propagate or swallow exception?

**QUESTION 6.2.4 (Combining Futures)**
"You need to fetch account details and transaction history (two async calls), then combine them. Show me CompletableFuture approach."

*What you're listening for:*
- thenCombine(): wait for both, combine results
- allOf(): wait for all futures
- anyOf(): wait for first future
- Order matters: async composition vs. sequential

**QUESTION 6.2.5 (Virtual Threads & CompletableFuture)**
"With Virtual Threads (Java 19+), does CompletableFuture become less necessary? How would you approach async in virtual thread world?"

*What you're listening for:*
- Virtual threads: lightweight, millions possible
- Can use synchronous code on virtual threads (cheap blocking)
- CompletableFuture still useful for batching, coordination
- But simpler models possible with virtual threads
- Are they aware of this shift?

---

## 6.3 Virtual Threads & Structured Concurrency

**QUESTION 6.3.1 (Virtual Threads Basics)**
"Explain Virtual Threads (Project Loom). How are they different from OS threads? What problem do they solve?"

*What you're listening for:*
- Virtual threads: millions possible (vs. thousands OS threads)
- OS thread: expensive (memory, context switching)
- Virtual thread: cheap (pooled on carrier threads)
- Can use blocking code (lock, read()) without fear of starving other tasks
- Simplifies async programming

**QUESTION 6.3.2 (Virtual Thread in Practice)**
"If you're on Java 21+, how would you use virtual threads for your payment processing? Show me code."

*What you're listening for:*
- Executors.newVirtualThreadPerTaskExecutor() for task submission
- try (var executor = Executors.newVirtualThreadPerTaskExecutor()) { executor.submit(task); }
- Simple blocking code: gateway.call() blocks virtual thread, not OS thread
- Scales to handle thousands of concurrent requests
- Have they actually used this?

**QUESTION 6.3.3 (Structured Concurrency)**
"Explain Structured Concurrency. What problem does it solve vs. unstructured async?"

*What you're listening for:*
- Structured: parent task waits for children to complete
- Unstructured: fire-and-forget, hard to track completion
- StructuredTaskScope: create child tasks, wait for all
- Cancellation: canceling parent cancels children
- Better control and visibility

**QUESTION 6.3.4 (Virtual Thread Limitations)**
"Are there downsides to virtual threads? When would they NOT be a good fit?"

*What you're listening for:*
- CPU-bound work: virtual threads don't help (need actual parallelism)
- Pinning: if blocked on synchronized (OS thread pinned), defeats purpose
- ReentrantLock better than synchronized
- Not a cure-all, understand workload

---

## 6.4 Concurrency Pitfalls

**QUESTION 6.4.1 (Race Conditions)**
"Show me code with a race condition. How would you detect it? How would you fix it?"

*What you're listening for:*
- Example: counter++ in multiple threads (not atomic)
- Detection: ThreadSanitizer, happens-before analysis
- Fix: synchronize, use AtomicInteger, etc.
- Testing concurrent code: hard! (non-deterministic)

**QUESTION 6.4.2 (Visibility Issues)**
"Explain visibility in concurrent programming. A thread changes a variable, another thread doesn't see the change. Why? How do you fix it?"

*What you're listening for:*
- Happens-before: memory model defines when writes visible
- Volatile: every read/write to volatile is visible
- Synchronized: release/acquire semantics
- Lock: acquire lock ensures seeing previous writes
- Without these: thread sees stale value from cache

**QUESTION 6.4.3 (Liveness Hazards)**
"Explain liveness hazards: deadlock, livelock, starvation. Give examples."

*What you're listening for:*
- Deadlock: threads blocked, waiting on each other
- Livelock: threads keep running but make no progress (retry loops)
- Starvation: thread never gets CPU/lock (lower priority threads starved)
- Prevention strategies differ for each

---

# SECTION 7: SPRING BOOT & MICROSERVICES

## 7.1 Spring Architecture

**QUESTION 7.1.1 (Spring Core & IoC)**
"Explain Spring's IoC container. How does it manage beans? Show me a simple Spring configuration."

*What you're listening for:*
- Bean: object managed by Spring container
- Lifecycle: instantiate, populate properties, initialize (init-method), use, destroy (destroy-method)
- Configuration: XML, annotations (@Component, @Bean), Java config
- Wiring: automatic, constructor, setter, field injection
- Scope: singleton (default), prototype, request, session

**QUESTION 7.1.2 (AOP - Aspect-Oriented Programming)**
"Explain AOP. Show me how you'd use Spring AOP for cross-cutting concerns like logging, security."

*What you're listening for:*
- AOP: separate cross-cutting concerns (logging, auth, transaction management)
- Aspects, Pointcuts, Advice (before, after, around)
- @Aspect, @Around annotation
- Example: logging aspect wraps all service methods
- Alternative: interceptors, filters

**QUESTION 7.1.3 (Spring Data JPA)**
"You've used Spring Data JPA extensively. Explain Spring Data JPA. How does it reduce boilerplate?"

*What you're listening for:*
- Repository interface extends JpaRepository
- CRUD methods generated automatically
- Query methods: findByXxx, findByXxxAndYyy (parsed from method name)
- @Query: custom queries (JPQL, SQL)
- Pagination, sorting built-in
- Problems: N+1 queries, lazy loading issues

**QUESTION 7.1.4 (Spring Security)**
"In your microservices, how do you secure APIs? Show me how you'd implement OAuth2 and JWT in Spring Security."

*What you're listening for:*
- SecurityFilterChain: configure which endpoints need auth
- @EnableGlobalMethodSecurity: method-level security
- OAuth2: @Bean ServerOAuth2AuthorizedClientRepository
- JWT: custom filter to validate JWT token
- RBAC: @PreAuthorize("hasRole('ADMIN')")

---

## 7.2 Microservices Design

**QUESTION 7.2.1 (Service Boundaries)**
"How do you decide service boundaries? In your payment system, would you have PaymentService, AccountService, FraudService as separate services? Why?"

*What you're listening for:*
- Service per domain (domain-driven design)
- Ownership: one team owns one service
- Database per service: loose coupling
- API contracts between services
- Trade-off: complexity vs. scalability

**QUESTION 7.2.2 (Service Communication)**
"Services need to communicate. Synchronous (REST) or asynchronous (Kafka)? When do you use each? Show me examples from your system."

*What you're listening for:*
- Synchronous: REST, gRPC (immediate response, tight coupling)
- Asynchronous: Kafka, RabbitMQ (eventual consistency, loose coupling)
- Payment initiated (sync), settlement processed (async)
- Trade-off: complexity vs. coupling
- Have they dealt with distributed transactions?

**QUESTION 7.2.3 (API Versioning)**
"Your payment API is v1. You need to add a new field. How do you version your API without breaking existing clients?"

*What you're listening for:*
- URL versioning: /v1/payments, /v2/payments
- Header versioning: Accept: application/vnd.payments.v2+json
- Backward compatibility: v1 clients must still work
- Graceful deprecation: sunset v1 over time
- Database schema migration: support both old and new fields

**QUESTION 7.2.4 (Service Resilience)**
"One service is slow. How do you prevent it from slowing down entire system? Show me circuit breaker + timeout + retry strategy."

*What you're listening for:*
- Timeout: long requests time out quickly
- Circuit breaker: don't keep calling failing service
- Retry: retry with backoff for transient failures
- Bulkhead: separate thread pools for different services
- Fallback: return cached/default value on failure

---

## 7.3 Event-Driven Architecture

**QUESTION 7.3.1 (Event-Driven vs. Request-Response)**
"Compare event-driven architecture with traditional request-response. When would you use each?"

*What you're listening for:*
- Request-response: immediate, tight coupling
- Event-driven: loose coupling, eventual consistency
- Example: Payment initiated (event) -> multiple listeners (audit, notification, fraud check)
- Trade-off: complexity vs. coupling
- Kafka for high-throughput, RabbitMQ for simpler needs

**QUESTION 7.3.2 (Event Sourcing)**
"Explain Event Sourcing. How is it different from CRUD? Have you implemented it?"

*What you're listening for:*
- Event Sourcing: store events, rebuild state from events
- Traditional: update database directly (destructive)
- Event Sourcing: immutable event log, replay for current state
- Advantages: audit trail, temporal queries, replay bugs
- Disadvantages: complexity, eventual consistency
- Example: Payment state changes logged as events

**QUESTION 7.3.3 (CQRS - Command Query Responsibility Segregation)**
"Explain CQRS. How does it complement Event Sourcing?"

*What you're listening for:*
- CQRS: separate models for command (write) and query (read)
- Command side: validates, produces events
- Query side: read model, optimized for queries
- Eventual consistency: query catches up to command
- Scaling: can scale read/write independently
- Complexity: eventual consistency, debugging harder

**QUESTION 7.3.4 (Kafka in Production)**
"You use Kafka for ACH processing. Walk me through architecture: producers, topics, consumers, partitions. How do you handle failures?"

*What you're listening for:*
- Topics: ACH-payments, ACH-settlements
- Producers: emit NACHA file events
- Consumers: process events (process, settle, notify)
- Partitions: parallelism, ordering within partition
- Consumer groups: multiple consumers share load
- Failure handling: dead letter queue, manual commits on success
- Idempotency: process event twice safely

---

# SECTION 8: FOLLOW-UP & PROBE QUESTIONS

## 8.1 Red Flag Patterns

Use these if answers seem canned or generic:

**Red Flag 1: Vague on LLM Experience**
- Response: "We use LLMs for intelligent recommendations"
- Probe: "Which model did you use? What was the cost? What was latency?"
- Red flag: Can't answer specifics, or pivots to generic

**Red Flag 2: No Production Failure Stories**
- Response: Lists technologies without incident
- Probe: "What broke in production? How did you debug?"
- Red flag: Shrugs or says "nothing broke"

**Red Flag 3: Textbook Java Answers**
- Response: Answers match Wikipedia, no personal experience
- Probe: "Show me code you wrote. What problem were you solving?"
- Red flag: Struggles to relate concept to real code

**Red Flag 4: No Design Trade-offs**
- Response: "We use microservices because they're better"
- Probe: "What was the complexity cost? When did it hurt?"
- Red flag: Doesn't mention trade-offs

**Red Flag 5: Uncomfortable with Concurrency**
- Response: Hand-wavy on CompletableFuture or virtual threads
- Probe: "Write pseudocode for a concurrent operation"
- Red flag: Can't translate concepts to code

---

## 8.2 Deep Dive Templates

Use these for follow-up based on their responses:

**Template 1: Zoom In on a Claim**
- Their claim: "We implemented Agentic AI workflows with LangChain"
- Follow-up: "Pick one concrete workflow. Start with what triggered it, what the agent did step-by-step, what tools it called, what happened when it failed."
- Purpose: Separate real from imagined

**Template 2: Trade-off Probe**
- Their claim: "We migrated to microservices"
- Follow-up: "Operationally, what became harder? Debugging? Deployment? Consistency?"
- Purpose: Shows depth and maturity

**Template 3: Scaling Question**
- Their claim: "We built a high-throughput system"
- Follow-up: "What was the bottleneck? CPU? Memory? Disk I/O? Network? How did you find it?"
- Purpose: Shows understanding of performance

**Template 4: Concurrency Challenge**
- If they mention async/concurrent work: "Walk me through a bug you fixed in concurrent code. What was the symptom? How did you debug it?"
- Purpose: Concurrency bugs are hard; real experience shows

**Template 5: Technology Choice**
- Their claim: "We chose technology X"
- Follow-up: "What was the alternative? Why not technology Y? What convinced you X was right?"
- Purpose: Shows thoughtfulness, not just resume-building

---

## 8.3 Real-Time Analysis Guide

During the interview, use this to evaluate responses:

**RATING CRITERIA**

| Aspect | Depth Signal | Surface Signal | Red Flag |
|--------|--------------|-----------------|----------|
| **AI/LLM** | Names specific models, costs, latencies, guardrails, failures | Lists tech stack, vague on integration | Can't articulate LLM API or guardrails |
| **Core Java** | Code examples, edge cases, performance implications | Textbook definitions, generic answers | Memorized wrong facts, contradictions |
| **Design Patterns** | Real code, why chosen, trade-offs, when NOT to use | Lists pattern names, theoretical examples | Confuses patterns, can't show usage |
| **SOLID** | Refactoring examples, DI in Spring context, violations caught | Definitions, textbook examples | Conflates principles, no application examples |
| **Concurrency** | Specific bugs faced, debugging tools, race condition detection | Safe answers (use synchronized), generic | Uncomfortable discussing, hand-wavy |
| **Microservices** | Operational complexity, consistency challenges, lessons learned | Technology list, architecture diagram | No awareness of complexity |

**SCORING MATRIX**

- **9-10**: Specific examples, trade-offs understood, production experience evident
- **7-8**: Good understanding, some hand-wavy on details
- **5-6**: Surface-level knowledge, resume vocabulary
- **3-4**: Confusion, inconsistencies, memorized wrong facts
- **1-2**: Doesn't understand fundamental concept

---

**DECISION GUIDE**

- **Hire if**: Scores 7+ across AI/LLM, Core Java, Concurrency, SOLID. Specific examples and trade-offs evident.
- **Maybe if**: Scores 6-7. Verify gaps in interviews before decision.
- **Pass if**: Scores 5- on AI/LLM or Concurrency. Foundation too weak or resume padded.

---

## INTERVIEW FLOW (Suggested Order)

1. **Warm-up (5 min)**: Background, current role, favorite project
2. **AI/LLM Deep Dive (15 min)**: Verify resume claims [Start with 1.2.1, follow patterns above]
3. **Core Java (15 min)**: Spot-check fundamentals [Sample 2.1-2.4]
4. **Design Patterns (10 min)**: Real application [Sample 5.1-5.4]
5. **Concurrency (10 min)**: Critical for Java roles [Section 6]
6. **SOLID (5 min)**: Quick probe on principles [Sample 4.1-4.5]
7. **System Design (10 min)**: Bring it together [Section 7]

**Total: 70 minutes**

---

## NOTES FOR LIVE INTERVIEW

This document is designed for real-time reference. During the interview:

1. Copy-paste questions from relevant section
2. Listen carefully for specific examples (good sign) vs. generic answers (bad sign)
3. When you hear a claim, follow Template 2-5 (Deep Dive Templates)
4. Take mental note of red flags from Section 8.1
5. Score using the matrix in 8.3 after each section
6. At end, refer to Decision Guide

**I'll be on standby during the interview. Forward me:**
- Question asked
- Their response (as much as you can capture)
- Your gut feel
- Ask: "Should I probe deeper?" or "Any follow-up you'd suggest?"

Good luck!

