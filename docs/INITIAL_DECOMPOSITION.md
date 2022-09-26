# Initial decomposition tips

Start test driven development together. 

E.g. start writing some business case

## Step 1

```java
@Autowired
private InterviewerService interviewerService;

@Test
void interviewerSlotMainScenario() {
    var slot = interviewerService.createSlot();
    assertThat(slot).isNotNull();
}
```

It is not compiled now, so just Alt+Enter on each red-highlited place and satisfy the compiler by:

* creating `InterviewerService.java` service
* creating `InterviewerTimeSlot.java` POJO
* adding `createSlot()` method, returning some empty `new InterviewerTimeSlot()`
* test should be green now

## Step 2

What you think the simpliest end elegant way to pass time slot parameters? Maybe like this:

```java
    var slot = interviewerService.createSlot(
        DayOfWeek.FRIDAY,
        LocalTime.of(9, 0), // 09:00
        LocalTime.of(17, 0) // 17:00       
    );
}
```

Or you might prefer this, because it is easier to get from input form:

```java
    var slotForm = InterviewerSlotForm.builder()
        .from("9:00")
        .to("17:00");
    var slot = interviewerService.createSlot(slotForm);
}
```

As well, make the test green by adding InterviewerSlotForm (in second case with @Builder annotation of lombok).

## Go ahead

Continue for all most essential business cases.

**Do not go into details** now, just 

* key **services** classes (or interfaces with stub implementations...
* with **essential methods** and... 
* key **business POJOs**

## Commit and create the Pull Request

At the final step: create essential PR `:rocket: Create essential stubs`

