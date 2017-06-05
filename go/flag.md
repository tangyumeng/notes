### flag


flag 包用来实现命令行工具


#### 基础用法：
1.
 用 flag.String() 、flag.Bool(), flag.Int() 等定义变量

```
import "flag"
var ip = flag.Int("flagname", 1234, "help message for flagname") 
```
上面代码定一个 integer 类型标记, 存放在变量ip 中, 类型为 *int。

一个完整的示例如下  

flag_test.go

```
package main
import (
	"flag"
	"fmt"
)
var ip *int

func init() {
	ip = flag.Int("flagname", 1234, "help message for flagname")
}

func main() {
	flag.Parse()
	fmt.Println(*ip)
}

```

终端执行 `go run flag_test.go` 输出结果为 `1234`  
终端执行 `go run flag_test.go -flagname 10` 输出结果为 `10`  
终端执行 `go run flag_test.go -flagname=15` 输出结果为 `15`  
注意 其中标记 `-flagname 10` `-flagname=15` 主要区别是 bool 类型的变量只能使用第二种形式  
2. 使用 Var() 方法把标记绑定到变量上，上面的例子可以改写成

```  
package main

import (
	"flag"
	"fmt"
)

var iv int

func init() {
	flag.IntVar(&iv, "flagname", 1234, "help message for flagname")
}

func main() {
	flag.Parse()
	fmt.Println(iv)
}
```

#### 高级用法

##### 子命令
当实现多个功能、并且每个功能都有一个特定的标记、参数集时比较有用。比如：

```  
git commit -m "message"  
git push
```

`flag.FlagSet` 可以实现子命令。

test_flagset.go

```  
package main

import (
	"flag"
	"fmt"
	"os"
	"strings"
)

func main() {
	// Subcommands
	countCommand := flag.NewFlagSet("count", flag.ExitOnError)
	listCommand := flag.NewFlagSet("list", flag.ExitOnError)

	// Count subcommand flag pointers
	// Adding a new choice for --metric of 'substring' and a new --substring flag
	countTextPtr := countCommand.String("text", "", "Text to parse. (Required)")
	countUniquePtr := countCommand.Bool("unique", false, "Measure unique values of a metric.")

	// List subcommand flag pointers
	listTextPtr := listCommand.String("text", "", "Text to parse. (Required)")
	listMetricPtr := listCommand.String("metric", "chars", "Metric <chars|words|lines>. (Required)")
	listUniquePtr := listCommand.Bool("unique", false, "Measure unique values of a metric.")

	// Verify that a subcommand has been provided
	// os.Arg[0] is the main command
	// os.Arg[1] will be the subcommand
	if len(os.Args) < 2 {
		fmt.Println("list or count subcommand is required")
		os.Exit(1)
	}

	// Switch on the subcommand
	// Parse the flags for appropriate FlagSet
	// FlagSet.Parse() requires a set of arguments to parse as input
	// os.Args[2:] will be all arguments starting after the subcommand at os.Args[1]
	switch os.Args[1] {
	case "list":
		listCommand.Parse(os.Args[2:])
	case "count":
		countCommand.Parse(os.Args[2:])
	default:
		flag.PrintDefaults()
		os.Exit(1)
	}

	// Check which subcommand was Parsed using the FlagSet.Parsed() function. Handle each case accordingly.
	// FlagSet.Parse() will evaluate to false if no flags were parsed (i.e. the user did not provide any flags)
	if listCommand.Parsed() {
		// Required Flags
		if *listTextPtr == "" {
			listCommand.PrintDefaults()
			os.Exit(1)
		}
		//Choice flag
		metricChoices := map[string]bool{"chars": true, "words": true, "lines": true}
		if _, validChoice := metricChoices[*listMetricPtr]; !validChoice {
			listCommand.PrintDefaults()
			os.Exit(1)
		}

		if *listMetricPtr == "words" {
			removePunctuation := func(r rune) rune {
				if strings.ContainsRune(".,:;?", r) {
					return -1
				} else {
					return r
				}
			}
			s := strings.Map(removePunctuation, *listTextPtr)
			words := strings.Fields(s)
			for i, word := range words {
				fmt.Println(i, " => ", word)
			}
		}

		// Print
		fmt.Printf("textPtr: %s, metricPtr: %s, uniquePtr: %t\n", *listTextPtr, *listMetricPtr, *listUniquePtr)
	}

	if countCommand.Parsed() {
		// Required Flags
		if *countTextPtr == "" {
			countCommand.PrintDefaults()
			os.Exit(1)
		}

		//Choice flag
		metricChoices := map[string]bool{"chars": true, "words": true, "lines": true, "substring": true}
		if _, validChoice := metricChoices[*listMetricPtr]; !validChoice {
			countCommand.PrintDefaults()
			os.Exit(1)
		}
		//Print
		fmt.Printf("textPtr: %s, uniquePtr: %t\n", *countTextPtr, *countUniquePtr)
	}

}

```


执行 ` go run test_flagset.go list -text "beijing ,haiDian tang yumeng?" -metric words`

结果为：

```
0  =>  beijing
1  =>  haiDian
2  =>  tang
3  =>  yumeng
textPtr: beijing ,haiDian tang yumeng?, metricPtr: words, uniquePtr: false
```


参考教程 

- [go语言的flag包简单使用教程](http://qefee.com/2014/02/02/go语言的flag包简单使用教程/)