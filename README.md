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

