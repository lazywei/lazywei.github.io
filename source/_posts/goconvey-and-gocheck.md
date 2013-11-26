title: GoConvey and Gocheck
date: 2013-11-25 19:12:08
tags: ["Golang", "Testing"]
---

I currently use [GoConvey](http://smartystreets.github.io/goconvey/) and [Gocheck](http://labix.org/gocheck) as my testing framework.

## GoConvey

GoConvey runs a HTTP server which has a really beautiful Web UI for reviewing testing results. The most important is that it will re-run the tests every time you change and save the files. In other words, it will watch any change of you files and run tests when need.

## Gocheck

Though GoConvey has a nice Web UI, it doesn't provide something like `SetupTest()`, which will be used to set common settings and will be called in every test case. Fortunately, there is a richer testing framework, Gocheck. Gocheck provides many useful features (e.g. fixtures, useful assertions and checks), and can be integrated with GoConvy.

## Examples

Here are some simple examples that how to use these two wonderful testing framework together. You should run `goconvey` in terminal and open `http://localhost:8080` first.

- Use GoConvey's `Convey` and Gocheck's `Suite`:

```go
import (
	"testing"
	"github.com/smartystreets/goconvey/convey"
	. "launchpad.net/gocheck"
)

// Hook up gocheck into the "go test" runner.
func Test(t *testing.T) {
	TestingT(t)
}

var _ = Suite(&S{})

type S struct {
	intShouldBeFive int
}

func (self *S) SetUpTest(c *C) {
	intShouldBeFive = 5
}

func (self *S) TestIntShouldBeFive(c *C) {
	convey.Convey("Foo", c, func() {
		convey.So(5, convey.ShouldEqual, self.intShouldBeFive)
	})
}
```

- Use Gocheck's assertions, and use GoConvey's Web UI only:

```go
import (
	"testing"
	. "launchpad.net/gocheck"
)

// Hook up gocheck into the "go test" runner.
func Test(t *testing.T) {
	TestingT(t)
}

var _ = Suite(&S{})

type S struct {
	intShouldBeFive int
}

func (self *S) SetUpTest(c *C) {
	intShouldBeFive = 5
}

func (self *S) TestIntShouldBeFive(c *C) {
	c.Check(self.intShouldBeFive, Equals, 5, Commentf("Int should be five failed."))
}
```

You can also find the real world examples at [polydice/pulse](https://github.com/polydice/pulse).
