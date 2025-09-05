
# MongoDB Assignment Workbook

**Dataset:** `db.companies` (20 SDE1 hiring companies)

---

## Section A. CRUD Basics (40 Queries)


```bash
use workbook; 
```

1. Insert a new company document for "Tesla" in Bangalore with base = 29 LPA.

```bash
db.companies.insertOne({
  name: 'Tesla',
  location: 'Bangalore',
  salaryBand: {
    base: 29,
    bonus: 5,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 8,
    skills: ["DSA", "Java"],
    experience: "2-4 years"
  },
  interviewRounds: [
    { round: 1, type: "OA" },
    { round: 2, type: "DSA Interview" },
    { round: 3, type: "Technical Interview" },
    { round: 4, type: "HR" }
  ],
  benefits: ["Relocation", "WFH", "Health Insurance"],
  headcount: 2000
});
```

```bash
{
  acknowledged: true,
  insertedId: ObjectId('68b735e3ee68ec72b52d5cec')
}
```

2. Insert multiple new companies at once (add "Stripe" and "Coinbase").

```bash
db.companies.insertMany([
  {
    name: 'Stripe',
    location: 'New Delhi',
    salaryBand: { base: 35, bonus: 10, stock: 9 },
    hiringCriteria: {
      cgpa: 9,
      skills: ["DSA", "Python", "Distributed Systems"],
      experience: "2-3 years"
    },
    interviewRounds: [
      { round: 1, type: "OA" },
      { round: 2, type: "Technical" },
      { round: 3, type: "HR" }
    ],
    benefits: ["Relocation", "Training"],
    headCount: 1000
  },
  {
    name: 'Coinbase',
    location: 'Hyderabad',
    salaryBand: { base: 27, bonus: 12, stock: 7 },
    hiringCriteria: {
      cgpa: 6.0,
      skills: ["DSA", "Java"],
      experience: "0-1 years"
    },
    interviewRounds: [
      { round: 1, type: "Aptitude" },
      { round: 2, type: "Technical" },
      { round: 3, type: "HR" }
    ],
    benefits: ["Relocation", "Health"],
    headCount: 1000
  }
]);
```
```bash
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('68b736d2ee68ec72b52d5ced'),
    '1': ObjectId('68b736d2ee68ec72b52d5cee')
  }
}
```

3. Find all companies.
```bash
db.companies.find();
```
```bash
{
  _id: ObjectId('68b72bb1ee68ec72b52d5cd9'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 30,
    bonus: 5,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  headcount: 2000
}
...
```
4. Find one company (`findOne()`) in Bangalore.
```bash
db.companies.findOne({location: 'Bangalore'});
```
```bash
{
  _id: ObjectId('68b72bb1ee68ec72b52d5cd9'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 30,
    bonus: 5,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  headcount: 2000
}
```
5. Find companies offering base > 30 LPA.
```bash
db.companies.find({"salaryBand.base" : {$gt: 30}});
```
```bash
{
  _id: ObjectId('68b72bb1ee68ec72b52d5cdb'),
  name: 'Microsoft',
  location: 'Hyderabad',
  salaryBand: {
    base: 32,
    bonus: 6,
    stock: 15
  },
  hiringCriteria: {
    cgpa: 8,
    skills: [
      'DSA',
      'C#',
      'System Design'
    ],
    experience: '1-3 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'Technical'
    },
    {
      round: 3,
      type: 'Managerial'
    },
    {
      round: 4,
      type: 'HR'
    }
  ],
  benefits: [
    'Health Insurance',
    'Stock Options',
    'Gym'
  ],
  headcount: 3000
}
...
```
6. Find companies in Hyderabad.
```bash
db.companies.find({ location: "Hyderabad" }); 
```
```bash
{
  _id: ObjectId('68b72bb1ee68ec72b52d5cda'),
  name: 'Amazon',
  location: 'Hyderabad',
  salaryBand: {
    base: 28,
    bonus: 3,
    stock: 12
  },
  hiringCriteria: {
    cgpa: 7,
    skills: [
      'DSA',
      'OS',
      'C++'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'DSA Interview'
    },
    {
      round: 3,
      type: 'System Design'
    },
    {
      round: 4,
      type: 'HR'
    }
  ],
  benefits: [
    'WFH',
    'Stock Options'
  ],
  headcount: 5000
}
```
7. Find companies requiring CGPA >= 8.0.
```bash
db.companies.find({ "hiringCriteria.cgpa": { $gte: 8 } });
```
```bash
{
  _id: ObjectId('68b72bb1ee68ec72b52d5cdb'),
  name: 'Microsoft',
  location: 'Hyderabad',
  salaryBand: {
    base: 32,
    bonus: 6,
    stock: 15
  },
  hiringCriteria: {
    cgpa: 8,
    skills: [
      'DSA',
      'C#',
      'System Design'
    ],
    experience: '1-3 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'Technical'
    },
    {
      round: 3,
      type: 'Managerial'
    },
    {
      round: 4,
      type: 'HR'
    }
  ],
  benefits: [
    'Health Insurance',
    'Stock Options',
    'Gym'
  ],
  headcount: 3000
}
...
```
8. Find companies that list "System Design" in skills.
```bash
db.companies.find({"hiringCriteria.skills": "System Design"});
```
```bash
{
  _id: ObjectId('68b72bb1ee68ec72b52d5cdb'),
  name: 'Microsoft',
  location: 'Hyderabad',
  salaryBand: {
    base: 32,
    bonus: 6,
    stock: 15
  },
  hiringCriteria: {
    cgpa: 8,
    skills: [
      'DSA',
      'C#',
      'System Design'
    ],
    experience: '1-3 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'Technical'
    },
    {
      round: 3,
      type: 'Managerial'
    },
    {
      round: 4,
      type: 'HR'
    }
  ],
  benefits: [
    'Health Insurance',
    'Stock Options',
    'Gym'
  ],
  headcount: 3000
}
...
```
9. Find companies that offer "Relocation".
```bash
db.companies.find({"benefits": "Relocation"});
```
```bash
{
  _id: ObjectId('68b72bb1ee68ec72b52d5ce1'),
  name: 'Uber',
  location: 'Bangalore',
  salaryBand: {
    base: 27,
    bonus: 5,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7,
    skills: [
      'DSA',
      'Java',
      'Distributed Systems'
    ],
    experience: '1-3 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'System Design'
    },
    {
      round: 3,
      type: 'Technical'
    },
    {
      round: 4,
      type: 'HR'
    }
  ],
  benefits: [
    'Relocation',
    'Health Insurance'
  ],
  headcount: 1600
}
...
```
10. Find companies with stock >= 15 LPA.
```bash
db.companies.find({"salaryBand.stock": {gte: 15}});
```
```bash
{
  _id: ObjectId('68b72bb1ee68ec72b52d5cdb'),
  name: 'Microsoft',
  location: 'Hyderabad',
  salaryBand: {
    base: 32,
    bonus: 6,
    stock: 15
  },
  hiringCriteria: {
    cgpa: 8,
    skills: [
      'DSA',
      'C#',
      'System Design'
    ],
    experience: '1-3 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'Technical'
    },
    {
      round: 3,
      type: 'Managerial'
    },
    {
      round: 4,
      type: 'HR'
    }
  ],
  benefits: [
    'Health Insurance',
    'Stock Options',
    'Gym'
  ],
  headcount: 3000
}
```
11. Find companies with at least 4 interview rounds.
```bash
db.companies.find({"interviewRounds.3": {$exists: true}});
```
```bash
{
  _id: ObjectId('68b72bb1ee68ec72b52d5cda'),
  name: 'Amazon',
  location: 'Hyderabad',
  salaryBand: {
    base: 28,
    bonus: 3,
    stock: 12
  },
  hiringCriteria: {
    cgpa: 7,
    skills: [
      'DSA',
      'OS',
      'C++'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'DSA Interview'
    },
    {
      round: 3,
      type: 'System Design'
    },
    {
      round: 4,
      type: 'HR'
    }
  ],
  benefits: [
    'WFH',
    'Stock Options'
  ],
  headcount: 5000
}
...
```
12. Find companies with headcount > 5000.
```bash
db.companies.find({headcount: {$gt:5000}});
```
```bash
{
  _id: ObjectId('68b72bb1ee68ec72b52d5ce6'),
  name: 'Infosys',
  location: 'Mysore',
  salaryBand: {
    base: 6,
    bonus: 1,
    stock: 0
  },
  hiringCriteria: {
    cgpa: 6,
    skills: [
      'DSA',
      'Java'
    ],
    experience: '0-1 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Aptitude'
    },
    {
      round: 2,
      type: 'Technical'
    },
    {
      round: 3,
      type: 'HR'
    }
  ],
  benefits: [
    'Training',
    'Relocation'
  ],
  headcount: 10000
}
...
```
13. Insert a company with only `name` and `location`.
```bash
db.companies.insertOne({name: 'Tata', location: 'Pune'});
```
```bash
{
  acknowledged: true,
  insertedId: ObjectId('68b95fe0689b99bb49cddd1b')
}
```
14. Update Amazon’s bonus to 6.
```bash
db.companies.updateOne({name: 'Amazon'}, 
{$set: {"salaryBand.bonus": 6 }});
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
```
15. Update all companies in Hyderabad to add benefit = "Free Snacks".
```bash
db.companies.updateMany({location: 'Hyderabad'},
{$addToSet : {benefits: 'Free Snacks'} });
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 5,
  modifiedCount: 5,
  upsertedCount: 0
}
```
16. Add skill "Python" to Google’s criteria.
```bash
db.companies.updateOne(
  { name: "Google" }, 
  { $addToSet: { "hiringCriteria.skills": "Python" } }
);
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
```
17. Remove "Gym" from Microsoft benefits.
```bash
db.companies.updateOne({name: "Microsoft"},
{$pull : {'benefits: 'Gym'}});
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
```
18. Replace entire salaryBand for Netflix.
```bash
db.companies.updateOne({name: 'Netflix'},
{$set: {'salaryBand': {
base: 18,
bonus: 6,
stock:10}}});
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
```
19. Delete company "Infosys".
```bash
db.companies.deleteOne({name: 'Infosys'});
```
```bash
{
  acknowledged: true,
  deletedCount: 1
}
```
20. Delete all companies with base < 10.
```bash
db.companies.deleteMany({"salaryBand.base": {$lt : 10}});
```
```bash
{
  acknowledged: true,
  deletedCount: 1
}
```
21. Use `$set` to add a new field `isTopTier: true` for Google.
```bash
db.companies.updateOne({name:'Google'},
{$set: {isTopTier: true}});
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
```
22. Use `$inc` to increase Amazon’s stock by 2.
```bash
db.companies.updateOne({name: 'Amazon'},
{$inc: {'salaryBand.stock': 2}});
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
```
23. Use `$rename` to rename field `headcount` → `employeeCount`.
```bash
db.companies.updateMany({}, {$rename: {'headcount': 'employeeCount'}});
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 21,
  modifiedCount: 18,
  upsertedCount: 0
}
```
24. Use `$unset` to remove "bonus" from salaryBand.
```bash
db.companies.updateMany({}, {$unset: {'salaryBand.band': ''}});
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 21,
  modifiedCount: 20,
  upsertedCount: 0
}
```
25. Insert 5 dummy companies with minimal fields.
```bash
db.companies.insertMany([
  { name: "dummy1" },
  { name: "dummy2" },
  { name: "dummy3" },
  { name: "dummy4" },
  { name: "dummy5" }
])
```
```bash
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('68b9b9a1689b99bb49cddd33'),
    '1': ObjectId('68b9b9a1689b99bb49cddd34'),
    '2': ObjectId('68b9b9a1689b99bb49cddd35'),
    '3': ObjectId('68b9b9a1689b99bb49cddd36'),
    '4': ObjectId('68b9b9a1689b99bb49cddd37')
  }
}
```
26. Delete all dummy companies.
```bash
db.companies.deleteMany({name: {$regex : /^dummy/, $options: "i"}});
```
```bash
{
  acknowledged: true,
  deletedCount: 5
}s
```
27. Find all companies, project only `name` and `salaryBand`.
```bash
db.companies.find({}, {name: 1, salaryBand:1, _id: 0});
```
```bash
{
  name: 'Google',
  salaryBand: {
    base: 30,
    stock: 10
  }
}
{
  name: 'Amazon',
  salaryBand: {
    base: 28,
    stock: 12
  }
}
```
28. Find all companies, exclude `_id`.
```bash
db.companies.find({},{_id: 0});
```
```bash
{
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 30,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  headcount: 2000
}
...
```
29. Find only `name` and `benefits` for Microsoft.
```bash
db.companies.findOne(
  {name: 'Microsoft'}, 
  {name: 1, benefits: 1, _id: 0}
  );

```
```bash
{
  name: 'Microsoft',
  benefits: [
    'Health Insurance',
    'Stock Options',
    'Gym'
  ]
}
```
30. Count all companies in Bangalore.
```bash
db.companies.countDocuments({location: 'Bangalore'});
```
```bash
10
```
31. Count all companies requiring CGPA >= 7.0.
```bash
db.companies.countDocuments({'hiringCriteria.cgpa':{$gte: 7.0}});
```
```bash
1
```
32. Get distinct locations.
```bash
db.companies.distinct('location');
```
```bash
[
  'Bangalore',
  'Gurgaon',
  'Hyderabad',
  'Mumbai',
  'New Delhi',
  'Noida',
  'Pune'
]
```
33. Get distinct benefits offered.
```bash
db.companies.distinct('benefits');
```
```bash
[
  'Device Discounts',
  'Free Food',
  'Free Lunch',
  'Free Meals',
  'Free Streaming',
  'Gym',
  'Health',
  'Health Insurance',
  'Relocation',
  'Stock Options',
  'Training',
  'WFH'
]
```
34. Check if any company offers stock = 20.
```bash
db.companies.findOne({'salaryBand.stock' : 20}) !== null;
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1f'),
  name: 'Meta',
  location: 'Bangalore',
  salaryBand: {
    base: 35,
    stock: 20
  },
  hiringCriteria: {
    cgpa: 8.5,
    skills: [
      'DSA',
      'React',
      'System Design'
    ],
    experience: '1-3 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'DSA'
    },
    {
      round: 3,
      type: 'System Design'
    },
    {
      round: 4,
      type: 'Culture Fit'
    }
  ],
  benefits: [
    'Stock Options',
    'Health Insurance',
    'Free Meals'
  ],
  headcount: 1500
}
```
35. Insert company with nested object `{ perks: { transport: true } }`.
```bash
db.companies.updateOne(
  { name: "Swiggy" },
  { $set: { perks: { transport: true } } }
)
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
```
36. Push new round `{ round: 5, type: "CTO Interview" }` to Meta.
```bash
db.companies.updateOne({name:'Meta'}, 
{$addToSet: {'interviewRounds': 
{round: 5,
type: 'CTO Interview'}
}}
);
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
```
37. Pull "HR" round out of Amazon’s rounds.
```bash
db.companies.updateOne({name:'Amazon'}, 
{$pull: {'interviewRounds': {
type: 'HR'}}}
);
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
```
38. Add "Health Insurance" only if not present in Swiggy benefits.
```bash
db.companies.updateOne({name:'Swiggy'}, 
{$addToSet: {'benefits': 'Health Insurance'}}
);
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
```
39. Increase base salary by 2 for all companies in Bangalore.
```bash
db.companies.updateMany({'location':'Bangalore'}, {$inc: {'salaryBand.base': 2}});
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 10,
  modifiedCount: 10,
  upsertedCount: 0
}
```
40. Delete all companies in "Delhi" (if any exist).
```bash
db.companies.deleteMany({location: {$regex: /Delhi/}});
```
```bash
{
  acknowledged: true,
  deletedCount: 1
}
```
---

## Section B. Advanced Queries (60 Queries)

41. Find companies with base between 25–35.
```bash
db.companies.find({'salaryBand.base' : {$gt : 25 , $lt: 35}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  headcount: 2000
}
...
```
42. Find companies where stock < 10.
```bash
db.companies.find({'salaryBand.stock': {$lt : 10}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd22'),
  name: 'Adobe',
  location: 'Noida',
  salaryBand: {
    base: 26,
    stock: 8
  },
  hiringCriteria: {
    cgpa: 7,
    skills: [
      'DSA',
      'OOPS',
      'C++'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'Technical'
    },
    {
      round: 3,
      type: 'HR'
    }
  ],
  benefits: [
    'Health Insurance',
    'WFH'
  ],
  headcount: 1800
}
...
```
43. Find companies with bonus > 5 AND stock > 10.
```bash
db.companies.find({'salaryBand.bonus': {$gt : 5}, 
'salaryBand.stock': {$gt:10}}
);
```
```bash
# Empty. Since we removed bonus in previous queries.
```
44. Find companies with base >= 30 OR stock >= 12.
```bash
db.companies.find({
  $or: [
  {'salaryBand.base':{$gte: 30}},
  {'salaryBand.stock':{$gte: 12}}
  ]
});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  headcount: 2000
}
...
```
45. Find companies NOT requiring "OS".
```bash
db.companies.find({'hiringCriteria.skills': {$ne: 'OS'}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  headcount: 2000
}
...
```
46. Find companies requiring at least one skill from \["Java", "C++"].
```bash
db.companies.find({ "hiringCriteria.skills": { $in: ["Java", "C++"] } });
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  headcount: 2000
}
...
```
47. Find companies requiring BOTH "DSA" and "System Design".
```bash
db.companies.find({ "hiringCriteria.skills": { $all: ["DSA", "System Design"] } });
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  headcount: 2000
}
...
```
48. Find companies not offering WFH.
```bash
db.companies.find({'benefits': {$ne : 'WFH'}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1e'),
  name: 'Microsoft',
  location: 'Hyderabad',
  salaryBand: {
    base: 32,
    stock: 15
  },
  hiringCriteria: {
    cgpa: 8,
    skills: [
      'DSA',
      'C#',
      'System Design'
    ],
    experience: '1-3 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'Technical'
    },
    {
      round: 3,
      type: 'Managerial'
    },
    {
      round: 4,
      type: 'HR'
    }
  ],
  benefits: [
    'Health Insurance',
    'Stock Options',
    'Gym'
  ],
  headcount: 3000
}
...
```
49. Find companies with > 3 benefits.
```bash
db.companies.find({'benefits.3': {$exists : true}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  headcount: 2000
}
...
```
50. Find companies with exactly 4 interview rounds.
```bash
db.companies.find({'interviewRounds.3': {$exists: true},
'interviewRounds.4': {$exists: false}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  headcount: 2000
}
...
```
51. Find companies where employeeCount > 2000.
```bash
db.companies.find({'employeeCount': {$gt : 2000}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1d'),
  name: 'Amazon',
  location: 'Hyderabad',
  salaryBand: {
    base: 28,
    stock: 12
  },
  hiringCriteria: {
    cgpa: 7,
    skills: [
      'DSA',
      'OS',
      'C++'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'DSA Interview'
    },
    {
      round: 3,
      type: 'System Design'
    }
  ],
  benefits: [
    'WFH',
    'Stock Options'
  ],
  employeeCount: 5000
}
....
```
52. Find companies offering salaries in multiples of 5.
```bash
db.companies.find({ "salaryBand.base": { $mod: [5, 0] } });
```
```bash
# Empty. Since incremented Google by 2 in the previous queries.
```
53. Find companies where CGPA is in \[6.5, 7.0, 7.5].
```bash
db.companies.find({'hiringCriteria.cgpa' : { $in : [6.5, 7.0, 7.5]}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  employeeCount: 2000
}
...
```
54. Find companies not in Bangalore.
```bash
db.companies.find({'location': {$ne : 'Bangalore'}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1d'),
  name: 'Amazon',
  location: 'Hyderabad',
  salaryBand: {
    base: 28,
    stock: 12
  },
  hiringCriteria: {
    cgpa: 7,
    skills: [
      'DSA',
      'OS',
      'C++'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'DSA Interview'
    },
    {
      round: 3,
      type: 'System Design'
    }
  ],
  benefits: [
    'WFH',
    'Stock Options'
  ],
  headcount: 5000
}
...
```
55. Use regex to find skills ending in "Design".
```bash
db.companies.find({'hiringCriteria.skills': {$regex : /Design/}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  employeeCount: 2000
}
```
56. Use regex to find companies starting with "A".
```bash
db.companies.find({name: {$regex: /^A/}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1d'),
  name: 'Amazon',
  location: 'Hyderabad',
  salaryBand: {
    base: 28,
    stock: 12
  },
  hiringCriteria: {
    cgpa: 7,
    skills: [
      'DSA',
      'OS',
      'C++'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'DSA Interview'
    },
    {
      round: 3,
      type: 'System Design'
    }
  ],
  benefits: [
    'WFH',
    'Stock Options'
  ],
  employeeCount: 5000
}
...
```
57. Case-insensitive search for "amazon".
```bash
db.companies.find({name: {$regex:/amazon/, $options: "i"}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1d'),
  name: 'Amazon',
  location: 'Hyderabad',
  salaryBand: {
    base: 28,
    stock: 12
  },
  hiringCriteria: {
    cgpa: 7,
    skills: [
      'DSA',
      'OS',
      'C++'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'DSA Interview'
    },
    {
      round: 3,
      type: 'System Design'
    }
  ],
  benefits: [
    'WFH',
    'Stock Options'
  ],
  employeeCount: 5000
}
```
58. Find companies where `salaryBand.stock` exists.
```bash
db.companies.find( {"salaryBand.stock":{$exists: true}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  employeeCount: 2000
}
...
```
59. Find companies where `perks` does NOT exist.
```bash
db.companies.find({"perks": {$exists: false}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  employeeCount: 2000
}
...
```
60. Find companies where `salaryBand.base` is of type number.
```bash
db.companies.find({"salaryBand.base" : {$type: "number"}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  employeeCount: 2000
}
...
```
61. Sort companies by base ascending.
```bash
db.companies.find().sort({"salaryBand.base": 1});
```
```bash
{
  _id: ObjectId('68b9b8ec689b99bb49cddd32'),
  name: 'Tata',
  location: 'Pune'
}
...
```
62. Sort companies by base descending.
```bash
db.companies.find().sort({"salaryBand.base": -1});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd21'),
  name: 'Netflix',
  location: 'Mumbai',
  salaryBand: {
    base: 38,
    stock: 12
  },
  hiringCriteria: {
    cgpa: 7,
    skills: [
      'Java',
      'DSA',
      'Microservices'
    ],
    experience: '2-4 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Technical'
    },
    {
      round: 2,
      type: 'System Design'
    },
    {
      round: 3,
      type: 'Managerial'
    }
  ],
  benefits: [
    'Stock Options',
    'Free Streaming',
    'WFH'
  ],
  employeeCount: 800
}
....
```
63. Sort by bonus, then stock.
```bash
db.companies.find().sort({"salaryBand.bonus": 1, "salaryBand.stock": 1});
```
```bash
{
  _id: ObjectId('68b9b8ec689b99bb49cddd32'),
  name: 'Tata',
  location: 'Pune'
}
```
64. Limit results to top 5 highest base salary.
```bash
db.companies.find().sort({"salaryBand.base": -1}).limit(5);
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd21'),
  name: 'Netflix',
  location: 'Mumbai',
  salaryBand: {
    base: 38,
    stock: 12
  },
  hiringCriteria: {
    cgpa: 7,
    skills: [
      'Java',
      'DSA',
      'Microservices'
    ],
    experience: '2-4 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Technical'
    },
    {
      round: 2,
      type: 'System Design'
    },
    {
      round: 3,
      type: 'Managerial'
    }
  ],
  benefits: [
    'Stock Options',
    'Free Streaming',
    'WFH'
  ],
  employeeCount: 800
}
....
```
65. Skip first 5 and return next 5.
```bash
db.companies.find().sort({"salaryBand.base": -1}).skip(5).limit(5);
```
```bash
{
  _id: ObjectId('68b9b8cf689b99bb49cddd31'),
  name: 'Tesla',
  location: 'Bangalore',
  salaryBand: {
    base: 31,
    bonus: 5,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 8,
    skills: [
      'DSA',
      'Java'
    ],
    experience: '2-4 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'DSA Interview'
    },
    {
      round: 3,
      type: 'Technical Interview'
    },
    {
      round: 4,
      type: 'HR'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  employeeCount: 2000
}
....
```
66. Find company with maximum base salary.
```bash
db.companies.find().sort({"salaryBand.base": -1}).limit(1);
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd21'),
  name: 'Netflix',
  location: 'Mumbai',
  salaryBand: {
    base: 38,
    stock: 12
  },
  hiringCriteria: {
    cgpa: 7,
    skills: [
      'Java',
      'DSA',
      'Microservices'
    ],
    experience: '2-4 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Technical'
    },
    {
      round: 2,
      type: 'System Design'
    },
    {
      round: 3,
      type: 'Managerial'
    }
  ],
  benefits: [
    'Stock Options',
    'Free Streaming',
    'WFH'
  ],
  employeeCount: 800
}
```
67. Find company with minimum CGPA requirement.
```bash
db.companies.find().sort({"hiringCriteria.cgpa": 1}).limit(1);
```
```bash
{
  _id: ObjectId('68b9b8ec689b99bb49cddd32'),
  name: 'Tata',
  location: 'Pune'
}

```
68. Return first 3 companies alphabetically by name.
```bash
db.companies.find().sort({"name":1}).limit(3);
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd22'),
  name: 'Adobe',
  location: 'Noida',
  salaryBand: {
    base: 26,
    stock: 8
  },
  hiringCriteria: {
    cgpa: 7,
    skills: [
      'DSA',
      'OOPS',
      'C++'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'Technical'
    },
    {
      round: 3,
      type: 'HR'
    }
  ],
  benefits: [
    'Health Insurance',
    'WFH'
  ],
  employeeCount: 1800
}
...
```
69. Find companies with at least one "Technical" interview round.
```bash
db.companies.find({ "interviewRounds.type": "Technical" })
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1e'),
  name: 'Microsoft',
  location: 'Hyderabad',
  salaryBand: {
    base: 32,
    stock: 15
  },
  hiringCriteria: {
    cgpa: 8,
    skills: [
      'DSA',
      'C#',
      'System Design'
    ],
    experience: '1-3 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'Technical'
    },
    {
      round: 3,
      type: 'Managerial'
    },
    {
      round: 4,
      type: 'HR'
    }
  ],
  benefits: [
    'Health Insurance',
    'Stock Options',
    'Gym'
  ],
  employeeCount: 3000
}
...
```
70. Find companies where round 2 is "System Design".
```bash
db.companies.find({"interviewRounds.1.type": "System Design"});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd21'),
  name: 'Netflix',
  location: 'Mumbai',
  salaryBand: {
    base: 38,
    stock: 12
  },
  hiringCriteria: {
    cgpa: 7,
    skills: [
      'Java',
      'DSA',
      'Microservices'
    ],
    experience: '2-4 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Technical'
    },
    {
      round: 2,
      type: 'System Design'
    },
    {
      round: 3,
      type: 'Managerial'
    }
  ],
  benefits: [
    'Stock Options',
    'Free Streaming',
    'WFH'
  ],
  employeeCount: 800
}
...
```
71. Find companies where interviewRounds length > 3.
```bash
db.companies.find({"interviewRounds.3" : {$exists: true}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  employeeCount: 2000
}
...
```
72. Find companies where second benefit = "WFH".
```bash
db.companies.find({ "benefits.1": "WFH" })
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  employeeCount: 2000
}
```
73. Use `$all` to find companies requiring \["DSA", "Java"].
```bash
db.companies.find({"hiringCriteria.skills": {$all: ["DSA", "Java"]}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  employeeCount: 2000
}
...
```
74. Use `$elemMatch` for base > 25 and stock > 5 in salaryBand.
```bash
db.companies.find({ "salaryBand.base": { $gt: 25 }, "salaryBand.stock": { $gt: 5 } });
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  employeeCount: 2000
}
...
```
75. Use `$in` for location in \["Hyderabad", "Bangalore"].
```bash
db.companies.find({"location": {$in: ["Hyderabad", "Bangalore"]}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  employeeCount: 2000
}
...
```
76. Use `$nin` for location not in \["Mumbai", "Pune"].
```bash
db.companies.find({"location": {$nin: ["Mumbai", "Pune"]}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  employeeCount: 2000
}
...
```
77. Find company closest to base = 30.
```bash
db.companies.aggregate([
  { $project: { name: 1, base: "$salaryBand.base", diff: { $abs: { $subtract: ["$salaryBand.base", 30] } } } },
  { $sort: { diff: 1 } },
  { $limit: 2 }
]);
```
```bash
# Google's base is 32
{
  _id: ObjectId('68b9b8ec689b99bb49cddd32'),
  name: 'Tata',
  diff: null
}
{
  _id: ObjectId('68b9b816689b99bb49cddd24'),
  name: 'Uber',
  base: 29,
  diff: 1
}
```
78. Use `$not` to exclude base < 20.
```bash
db.companies.find({"salaryBand.base" : { $not : {$lt : 20}}});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  employeeCount: 2000
}
```
79. Use `$expr` to find companies where bonus < base/10.
```bash
db.companies.find({ $expr: { $lt: ["$salaryBand.bonus", { $divide: ["$salaryBand.base", 10] }] } })
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 10
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Managerial Interview'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  employeeCount: 2000
}
...
```
80. Use `$size` to find companies with exactly 2 benefits.
```bash
db.companies.find({"benefits": {$size : 2 }});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1d'),
  name: 'Amazon',
  location: 'Hyderabad',
  salaryBand: {
    base: 28,
    stock: 12
  },
  hiringCriteria: {
    cgpa: 7,
    skills: [
      'DSA',
      'OS',
      'C++'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'DSA Interview'
    },
    {
      round: 3,
      type: 'System Design'
    }
  ],
  benefits: [
    'WFH',
    'Stock Options'
  ],
  employeeCount: 5000
}
...
```
81. Project new field `totalComp = base+bonus+stock`.
```bash
db.companies.aggregate([
  { $project: { name: 1, totalComp: { $add: ["$salaryBand.base", "$salaryBand.bonus", "$salaryBand.stock"] } } }
]);
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  totalComp: null
}
...
```
82. Find companies where totalComp > 45.
```bash
db.companies.aggregate([
  { $project: { name: 1, totalComp: { $add: ["$salaryBand.base", "$salaryBand.bonus", "$salaryBand.stock"] } } },
  { $match: { totalComp: { $gt: 45 } } }
]);
```
```bash
{
  _id: ObjectId('68b9b8cf689b99bb49cddd31'),
  name: 'Tesla',
  totalComp: 46
}
```
83. Sort companies by totalComp descending.
```bash
db.companies.aggregate([
  { $project: { name: 1, totalComp: { $add: ["$salaryBand.base", "$salaryBand.bonus", "$salaryBand.stock"] } } },
  { $sort: { totalComp: -1 } }
])
```
```bash
{
  _id: ObjectId('68b9b8cf689b99bb49cddd31'),
  name: 'Tesla',
  totalComp: 46
}
...
```
84. Use `$mul` to multiply stock by 2.
```bash
db.companies.updateMany({}, {$mul : {"salaryBand.stock": 2}});
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 21,
  modifiedCount: 20,
  upsertedCount: 0
}
```
85. Use `$max` to ensure bonus >= 5.
```bash
db.companies.updateMany({}, {$max : { "salaryBand.bonus": 5}});
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 21,
  modifiedCount: 21,
  upsertedCount: 0
}
```
86. Use `$min` to cap base salary at 35.
```bash
db.companies.updateMany({},{$min : {"salaryBand.base": 35}});
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 21,
  modifiedCount: 3,
  upsertedCount: 0
}
```
87. Use `$addToSet` to ensure unique benefit "WFH".
```bash
db.companies.updateMany({}, {$addToSet: { benefits: "WFH"}});
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 21,
  modifiedCount: 13,
  upsertedCount: 0
}
```
88. Use arrayFilters to update only 3rd round = "Tech Screen".
```bash
db.companies.updateMany(
  { "interviewRounds.round": 3 },  // optional: filter to match fewer docs
  {
    $set: {
      "interviewRounds.$[elem].type": "Tech Screen"
    }
  },
  {
    arrayFilters: [
      { "elem.round": 3 }
    ]
  }
);

```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 20,
  modifiedCount: 0,
  upsertedCount: 0
}
```
89. Update multiple docs with `$currentDate` for `lastUpdated`.
```bash
db.companies.updateMany({}, { $currentDate: { lastUpdated: true } })
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 21,
  modifiedCount: 21,
  upsertedCount: 0
}
```
90. Delete companies without benefits field.
```bash
db.companies.deleteMany({ benefits: { $exists: false } })
```
```bash
{
  acknowledged: true,
  deletedCount: 0
}
```
91. Upsert: Update "Tesla" if exists, insert if not.
```bash
db.companies.updateOne(
  { name: "Tesla" },
  { $set: { location: "Bangalore" } },
  { upsert: true }
)
```
```bash
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 0,
  upsertedCount: 0
}
```
92. Find all companies but exclude "salaryBand".
```bash
db.companies.find({}, { salaryBand: 0 })
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Tech Screen'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  employeeCount: 2000,
  lastUpdated: 2025-09-05T07:57:11.905Z
}
```
93. Project only name and computed field `doubleStock = stock*2`.
```bash
db.companies.aggregate([
  { $project: { name: 1, doubleStock: { $multiply: ["$salaryBand.stock", 2] } } }
])
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  doubleStock: 40
}
...
```
94. Match companies whose name length = 6 using `$expr`.
```bash
db.companies.find({ $expr: { $eq: [{ $strLenCP: "$name" }, 6] } })
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 20,
    bonus: 5
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Tech Screen'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  employeeCount: 2000,
  lastUpdated: 2025-09-05T07:57:11.905Z
}
...
```
95. Query with `$mod`: base % 2 = 0.
```bash
db.companies.find({ "salaryBand.base": { $mod: [2, 0] } })
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 20,
    bonus: 5
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Tech Screen'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  employeeCount: 2000,
  lastUpdated: 2025-09-05T07:57:11.905Z
}
...
```
96. Query with `$where`: headcount > 2000.
```bash
db.companies.find({ employeeCount: { $gt: 2000 } });
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1d'),
  name: 'Amazon',
  location: 'Hyderabad',
  salaryBand: {
    base: 28,
    stock: 24,
    bonus: 5
  },
  hiringCriteria: {
    cgpa: 7,
    skills: [
      'DSA',
      'OS',
      'C++'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'DSA Interview'
    },
    {
      round: 3,
      type: 'Tech Screen'
    }
  ],
  benefits: [
    'WFH',
    'Stock Options'
  ],
  employeeCount: 5000,
  lastUpdated: 2025-09-05T07:57:11.905Z
}
```
97. Query using `$text` after creating text index on `skills`.
```bash
db.companies.createIndex({ "hiringCriteria.skills": "text" })
db.companies.find({ $text: { $search: "Java" } })
```
```bash
{
  _id: ObjectId('68b9b8cf689b99bb49cddd31'),
  name: 'Tesla',
  location: 'Bangalore',
  salaryBand: {
    base: 31,
    bonus: 5,
    stock: 20
  },
  hiringCriteria: {
    cgpa: 8,
    skills: [
      'DSA',
      'Java'
    ],
    experience: '2-4 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'DSA Interview'
    },
    {
      round: 3,
      type: 'Tech Screen'
    },
    {
      round: 4,
      type: 'HR'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  employeeCount: 2000,
  lastUpdated: 2025-09-05T07:57:11.905Z
}
....
```
98. Use collation for case-insensitive sort by name.
```bash
db.companies.find().collation({ locale: "en", strength: 2 }).sort({ name: 1 })
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd22'),
  name: 'Adobe',
  location: 'Noida',
  salaryBand: {
    base: 26,
    stock: 16,
    bonus: 5
  },
  hiringCriteria: {
    cgpa: 7,
    skills: [
      'DSA',
      'OOPS',
      'C++'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'Technical'
    },
    {
      round: 3,
      type: 'Tech Screen'
    }
  ],
  benefits: [
    'Health Insurance',
    'WFH'
  ],
  employeeCount: 1800,
  lastUpdated: 2025-09-05T07:57:11.905Z
}
...
```
99. Query with `$type: "array"` on benefits.
```bash
db.companies.find({ benefits: { $type: "array" } })
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1c'),
  name: 'Google',
  location: 'Bangalore',
  salaryBand: {
    base: 32,
    stock: 20,
    bonus: 5
  },
  hiringCriteria: {
    cgpa: 7.5,
    skills: [
      'DSA',
      'System Design',
      'Java'
    ],
    experience: '0-2 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'Online Assessment'
    },
    {
      round: 2,
      type: 'Technical Interview'
    },
    {
      round: 3,
      type: 'Tech Screen'
    },
    {
      round: 4,
      type: 'HR Round'
    }
  ],
  benefits: [
    'Relocation',
    'WFH',
    'Health Insurance'
  ],
  employeeCount: 2000,
  lastUpdated: 2025-09-05T07:57:11.905Z
}
...
```
100. Find companies where "Free Meals" is one of the benefits.
```bash
db.companies.find({"benefits":"Free Meals"});
```
```bash
{
  _id: ObjectId('68b9b816689b99bb49cddd1f'),
  name: 'Meta',
  location: 'Bangalore',
  salaryBand: {
    base: 35,
    stock: 40,
    bonus: 5
  },
  hiringCriteria: {
    cgpa: 8.5,
    skills: [
      'DSA',
      'React',
      'System Design'
    ],
    experience: '1-3 years'
  },
  interviewRounds: [
    {
      round: 1,
      type: 'OA'
    },
    {
      round: 2,
      type: 'DSA'
    },
    {
      round: 3,
      type: 'Tech Screen'
    },
    {
      round: 4,
      type: 'Culture Fit'
    },
    {
      round: 5,
      type: 'CTO Interview'
    }
  ],
  benefits: [
    'Stock Options',
    'Health Insurance',
    'Free Meals',
    'WFH'
  ],
  employeeCount: 1500,
  lastUpdated: 2025-09-05T07:57:11.905Z
}
...
```
---