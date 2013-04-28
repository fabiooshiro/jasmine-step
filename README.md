jasmine-step
============

Simplify your asynchronous tests

```javascript
describe("some feature", function(){
    it("should calculate max on server", function(){
        var done = false;
        runs(function(){
            new Criteria('Music')
                .projections(function(p){
                    p.max('time');
                })
                .success(function(response){
                    expect(response.length).toEqual(1);
                    expect(response[0]).toEqual(100);
                    done = true;
                })
            ;
        });
        waitsFor(function(){ return done; }, 'Test timeout', 10000);
    });
});
```

With step

```javascript
describe("some feature", function(){
    it("should calculate max on server", function(){
        step("call server side", function(done){
            new Criteria('Music')
                .projections(function(p){
                    p.max('time');
                })
                .success(function(response){
                    expect(response.length).toEqual(1);
                    expect(response[0]).toEqual(100);
                    done();
                })
            ;
        });
    });
});
```

Sync step

```javascript
describe("some feature", function(){
    it("should calculate max on server", function(){
        step("call server side", function(){ // without done
            new Criteria('Music')
                .projections(function(p){
                    p.max('time');
                })
                .success(function(response){
                    expect(response.length).toEqual(1);
                    expect(response[0]).toEqual(100);
                })
            ;
        });
    });
});
```

## Full docs:

```javascript
describe("Jasmine Step", function(){

    it("should waits for done result", function(){
        step("watch the counter", function(done){
            resetCounter();
            //equals waitsFor(function(){$('#counter').text() == '3s';}, 'x', 5000);
            done(function(){
                return $('#counter').text() == '3s';
            });
        });
    });

    it("should do sync and asynchronous step", function(){
        step("fill form", function(){
            $('[name=username]').val("jasmine");
        });
        step("watch the counter", function(done){
            resetCounter();
            done(function(){
                return $('#counter').text() == '3s';
            });
        });
    });

    it("should accept done without args", function(){
        step("call the serverside", function(done){
            $.ajax({url: 'http://server/rest', dataType: "json",
                success: function(data){
                    $('#serverResponse').text(data.msg);
                    done();
                }
            });
        });
        step("verify message received", function(){
            expect($('#serverResponse').text()).toBe("ok");
        });
    });
});
```
