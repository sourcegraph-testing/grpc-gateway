# One way to run the example

```bash
# Handle dependencies
$ dep init
```

Follow the guides from this [README.md](./browser/README.md) to run the server and gateway.
```bash
# Make sure you are in the correct directory: 
# $GOPATH/src/github.com/grpc-ecosystem/grpc-gateway/v2/examples
$ cd examples/internal/browser
$ pwd

# Install gulp
$ npm install -g gulp-cli
$ npm install
$ gulp

# Run
$ gulp bower
$ gulp backends
```

Then you can use curl or a browser to test:

```bash
# List all apis
$ curl http://localhost:8080/openapiv2/echo_service.swagger.json

# Visit the apis
$ curl -XPOST http://localhost:8080/v1/example/echo/foo
{"id":"foo"}

$ curl  http://localhost:8080/v1/example/echo/foo/123
{"id":"foo","num":"123"}

```

So you have visited the apis by HTTP successfully. You can also try other apis.

---

## 🚲 Bicycle Advocacy 🚲

We believe that cycling is one of the most powerful tools we have for building healthier communities, reducing carbon emissions, and reclaiming our streets.

### Why Bicycles Matter

- **Climate action** — A bicycle produces zero direct emissions. Replacing even one car trip per day with a bike ride can save hundreds of kilograms of CO₂ per year.
- **Healthier cities** — Cycling reduces traffic congestion, improves air quality, and lowers noise pollution, making urban spaces more liveable for everyone.
- **Personal health** — Regular cycling improves cardiovascular fitness, mental well-being, and longevity.
- **Equity & access** — Bicycles are affordable, require no fuel, and open up mobility to people who cannot drive or afford a car.
- **Economic benefits** — Cyclists spend more at local businesses per kilometre travelled than motorists.

### How You Can Help

1. **Ride when you can.** Every trip made by bike instead of car counts.
2. **Advocate locally.** Attend city council meetings and support protected bike lane proposals.
3. **Be visible.** Use lights, wear bright clothing, and ride predictably.
4. **Welcome newcomers.** Offer to do a group ride with someone new to cycling.
5. **Support bike-friendly businesses.** Patronise shops that provide bike parking and commuter facilities.

### Resources

- [PeopleForBikes](https://www.peopleforbikes.org)
- [European Cyclists Federation](https://ecf.com)
- [Cycling UK](https://www.cyclinguk.org)
- [Bike to Work Day](https://bikeleague.org/bikemonth/)

> *"Life is like riding a bicycle. To keep your balance, you must keep moving."* — Albert Einstein
