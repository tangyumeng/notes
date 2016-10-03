```golang

package main

import (
	"fmt"
	"github.com/jinzhu/gorm"
	_ "github.com/jinzhu/gorm/dialects/mysql"
	"time"
)

type Post struct {
	Id        int
	Content   string
	Author    string `sql:"not null"`
	Comments  []Comment
	CreatedAt time.Time
}

type Comment struct {
	Id        int
	Content   string
	Author    string `sql:"not null"`
	PostId    int
	CreatedAt time.Time
}

var Db *gorm.DB

// connect to the Db
func init() {
	var err error
	Db, err = gorm.Open("mysql", "root:root@/test?charset=utf8&parseTime=True&loc=Local")
	if err != nil {
		panic(err)
	}
	Db.AutoMigrate(&Post{}, &Comment{})
}

func main() {
	post := Post{Content: "Hello World!", Author: "tangyumeng"}
	fmt.Println(post) // [{1 Good post! Joe 1 2016-10-03 18:14:37 +0800 CST} {2 Good post! Joe 1 2016-10-03 18:14:37 +0800 CST}]

	// Create a post
	Db.Create(&post)
	fmt.Println(post) // {1 Hello World! tangyumeng [] 2016-10-03 18:11:43.714935926 +0800 CST}

	// Add a comment
	comment1 := Comment{Content: "Good post!", Author: "Joe"}
	comment2 := Comment{Content: "Good post!", Author: "Joe"}
	Db.Model(&post).Association("Comments").Append(comment1, comment2)

	// Get comments from a post
	var readPost Post
	Db.Where("author = ?", "tangyumeng").First(&readPost)
	var comments []Comment
	Db.Model(&readPost).Related(&comments)
	fmt.Println(comments) // {1 Good post! Joe 1 2015-04-13 11:38:50.920377 +0800 SGT}
}


```
