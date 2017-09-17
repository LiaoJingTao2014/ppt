[slide]
##order guest

db.order.insertMany([
    {reservedAt: ISODate("2017-09-15T14:00:00Z"), tableIds: "A", partySize:NumberInt(2), status: "complete", guestIds: [ObjectId("59be40be15da5010cc302a9f"), ObjectId("59be40be15da5010cc302aa0")]},
    {reservedAt: ISODate("2017-09-15T16:00:00Z"), tableIds: "B", partySize:NumberInt(1), status: "cancel", guestIds: [ObjectId("59be40be15da5010cc302aa2")]},
    {reservedAt: ISODate("2017-09-16T08:00:00Z"), tableIds: "C", partySize:NumberInt(2), status: "complete", guestIds: [ObjectId("59be40be15da5010cc302aa1"), ObjectId("59be40be15da5010cc302aa2")]},
    {reservedAt: ISODate("2017-09-16T17:00:00Z"), tableIds: "B", partySize:NumberInt(3), status: "complete", guestIds: [ObjectId("59be40be15da5010cc302a9e"), ObjectId("59be40be15da5010cc302aa0"),ObjectId("59be40be15da5010cc302a9d")]},
    {reservedAt: ISODate("2017-09-17T10:00:00Z"), tableIds: "D", partySize:NumberInt(2), status: "seat", guestIds: [ObjectId("59be40be15da5010cc302a9e"), ObjectId("59be40be15da5010cc302aa2")]},
    {reservedAt: ISODate("2017-09-17T15:00:00Z"), tableIds: "F", partySize:NumberInt(6), status: "reserve", guestIds: [ObjectId("59be40be15da5010cc302a9d"), ObjectId("59be40be15da5010cc302a9e"), 
    ObjectId("59be40be15da5010cc302a9f"), ObjectId("59be40be15da5010cc302aa0"), ObjectId("59be40be15da5010cc302aa1"), ObjectId("59be40be15da5010cc302aa2")]},
    ])


    
db.order.aggregate(
    [
        {$unwind: '$guestIds'}, 
        {$lookup: {
               from: "guest",
               localField: "guestIds",
               foreignField: "_id",
               as: "guestInformation"
                  }
        }, 
        {$group: {_id: {month: {$month: "$reservedAt"}, date: {$dayOfMonth: "$reservedAt"}, year: {$year: "$reservedAt"}}, 'orders': { $push: "$$ROOT" }}},
        {$unwind: "$orders"},
        {$sort:{_id: 1, reservedAt: 1}}
    ]
)

/* 1 createdAt:2017/9/17 下午5:30:38*/
{
	"_id" : ObjectId("59be40be15da5010cc302a9d"),
	"name" : "byron",
	"sex" : "M",
	"phone" : "114"
},

/* 2 createdAt:2017/9/17 下午5:30:38*/
{
	"_id" : ObjectId("59be40be15da5010cc302a9e"),
	"name" : "judith",
	"sex" : "F",
	"phone" : "114"
},

/* 3 createdAt:2017/9/17 下午5:30:38*/
{
	"_id" : ObjectId("59be40be15da5010cc302a9f"),
	"name" : "dony",
	"sex" : "M",
	"phone" : "114"
},

/* 4 createdAt:2017/9/17 下午5:30:38*/
{
	"_id" : ObjectId("59be40be15da5010cc302aa0"),
	"name" : "quinn",
	"sex" : "M",
	"phone" : "114"
},

/* 5 createdAt:2017/9/17 下午5:30:38*/
{
	"_id" : ObjectId("59be40be15da5010cc302aa1"),
	"name" : "ivin",
	"sex" : "M",
	"phone" : "114"
},

/* 6 createdAt:2017/9/17 下午5:30:38*/
{
	"_id" : ObjectId("59be40be15da5010cc302aa2"),
	"name" : "kyle",
	"sex" : "M",
	"phone" : "114"
}
http://www.jb51.net/article/87823.htm
使用聚合框架对集合中的文档进行变换和组合，可以用多个构件创建一个管道(pipeline)，用于对一连串的文档进行处理。这些构件包括筛选(filtering),投射(projecting),分组(grouping),排序(sorting),限制(limiting),跳过(skipping)。
