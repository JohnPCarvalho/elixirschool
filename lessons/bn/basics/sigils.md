%{
  version: "1.0.2",
  title: "সিজিল",
  excerpt: """
  এই অধ্যায়ে আমরা সিজিল দিয়ে কাজ করা ও সিজিল তৈরি করা নিয়ে আলোচনা করব।
  """
}
---

## সিজিল পরিচিতি

লিটেরাল দিয়ে কাজ করার একটি ভিন্ন ধরনের সিনট্যাক্স দিয়ে থাকে এলিক্সির, তা হলো সিজিল। 
সিজিল শুরু হয় টিল্ডা `~` অপারেটর দিয়ে যার পরে একটি ক্যারেক্টার আসে। 
এলিক্সির নিজে কিছু বীল্ট-ইন সিজিল দেয়, তবে প্রয়োজন অনুসারে নিজস্ব সিজিল তৈরী করেও এলিক্সির ল্যাঙ্গুয়েজকে এক্সটেন্ড তথা বর্ধিত করা সম্ভব।

বীল্ট-ইন সিজিলের একটি লিস্ট নীচে দেয়া হয়েছেঃ 

  - `~C` এস্কেপ অথবা ইন্টারপোলেশান **ছাড়া** ক্যারেক্টার লিস্ট জেনারেট করে
  - `~c` এস্কেপ এবং ইন্টারপোলেশান **সহ** ক্যারেক্টার লিস্টজেনারেট করে
  - `~R` এস্কেপ অথবা ইন্টারপোলেশান **ছাড়া** রেগুলার এক্সপ্রেশান জেনারেট করে
  - `~r` এস্কেপ এবং ইন্টারপোলেশান **সহ** রেগুলার এক্সপ্রেশান জেনারেট করে
  - `~S` এস্কেপ অথবা ইন্টারপোলেশান **ছাড়া** স্ট্রিং জেনারেট করে
  - `~s` এস্কেপ এবং ইন্টারপোলেশান **সহ** স্ট্রিং জেনারেট করে
  - `~W` এস্কেপ অথবা ইন্টারপোলেশান **ছাড়া** ওয়ার্ড লিস্ট জেনারেট করে
  - `~w` এস্কেপ এবং ইন্টারপোলেশান **সহ** ওয়ার্ড লিস্ট জেনারেট করে
  - `~N` নাইভ ডেট টাইম `NaiveDateTime` স্ট্রাক্ট জেনারেট করে
  - `~U` `DateTime` স্ট্রাক্ট জেনারেট করে (এলিক্সির ১.৯.০ হতে)

আর ডেলিমিটারের লিস্ট হলঃ 

  - `<...>` এক জোড়া পয়েন্টী ব্র্যাকেট
  - `{...}` এক জোড়া কার্লী ব্র্যাকেট
  - `[...]` এক জোড়া স্কোয়ার ব্র্যাকেট
  - `(...)` এক জোড়া প্যারেনথেসিস
  - `|...|` এক জোড়া পাইপ
  - `/.../` এক জোড়া ফরওয়ার্ড স্ল্যাশ
  - `"..."` এক জোড়া ডাবল কোট
  - `'...'` এক জোড়া সিংগল কোট

### কার লিস্ট 

`~c` আর `~C` দুটোই ক্যারেক্টার লিস্ট তৈরি করে।
যেমনঃ

```elixir
iex> ~c/2 + 7 = #{2 + 7}/
'2 + 7 = 9'

iex> ~C/2 + 7 = #{2 + 7}/
'2 + 7 = \#{2 + 7}'
```

লক্ষ্য করুন `~c` ইন্টারপোলেশন এর মাধ্যমে ক্যালকুলেশন করে থাকে কিন্তু `~C` তা করে না। 
পরবর্তীতে আমরা দেখব যে এরকম আপারকেইস ও লোয়ারকেইস বীল্ট-ইন সিজিলে প্রায়ই ব্যবহৃত হয়।

### রেগুলার এক্সপ্রেশান 

`~r` ও `~R` সিজিল দিয়ে রেগুলার এক্সপ্রেশান সৃষ্টি করা হয়।  
আমরা হয় সরাসরি অথবা `Regex` ফাংশনে তা ব্যবহার করি।
উদাহরণঃ  

```elixir
iex> re = ~r/elixir/
~r/elixir/

iex> "Elixir" =~ re
false

iex> "elixir" =~ re
true
```

দেখা যাচ্ছে যে প্রথমে আমরা সমতা টেস্ট করছি, অর্থাৎ এলিক্সির রেগুলার এক্সপ্রেশানের সাথে ম্যাচ করছে না। 
এর কারণ এটি ক্যাপিটালাইজড অর্থাৎ বড় হাতের অক্ষরের। 
যেহেতু এলিক্সির পার্ল কম্প্যাটিবল রেগুলার এক্সপ্রেশান (PCRE) সাপোর্ট করে, আমরা সিজিলের শেষে `i` যোগ করে সিজিলকে জানিয়ে দিতে পারি যে কেইস সেনসিটিভ হওয়ার দরকার নেই। 

```elixir
iex> re = ~r/elixir/i
~r/elixir/i

iex> "Elixir" =~ re
true

iex> "elixir" =~ re
true
```

আবার এলিক্সির [Regex](https://hexdocs.pm/elixir/Regex.html) এপিআই দিয়ে থাকে যা এরল্যাঙ্গের রেগুলার এক্সপ্রেশান লাইব্রেরীর উপর ভিত্তি করে তৈরি। 
চলুন `Regex.split/2` এর সাথে সিজিল ব্যবহার করিঃ 

```elixir
iex> string = "100_000_000"
"100_000_000"

iex> Regex.split(~r/_/, string)
["100", "000", "000"]
```

`"100_000_000"` কে আন্ডারস্কোরের মাধ্যমে স্প্লিট করা হয়েছে  `~r/_/` সিজিলের সাহায্যে। 
`Regex.split` ফাংশন, একটি লিস্ট রিটার্ন করে।

### স্ট্রিং 

স্ট্রিং ডাটা আপনি পাবেন সিজিল `~s` ও `~S` এর মাধ্যমে।
উদাহরণঃ

```elixir
iex> ~s/the cat in the hat on the mat/
"the cat in the hat on the mat"

iex> ~S/the cat in the hat on the mat/
"the cat in the hat on the mat"
```

এখানে পার্থক্য কোথায়? পার্থক্যটা অনেকটাই একটু আগে দেখা ক্যারাক্টার লিস্ট সিজিল এর মতোই। 
এর উত্তর হলো, ইন্টারপোলেশন এবং এস্কেপ সিকুয়েন্স এর ব্যবহার। 
আরেকটি উদাহরণ দেখা যাকঃ 

```elixir
iex> ~s/welcome to elixir #{String.downcase "SCHOOL"}/
"welcome to elixir school"

iex> ~S/welcome to elixir #{String.downcase "SCHOOL"}/
"welcome to elixir \#{String.downcase \"SCHOOL\"}"
```

### ওয়ার্ড লিস্ট 

ওয়ার্ড লিস্ট বিভিন্ন সময়েই আমাদের কাজে লাগে।  
এটা সময় ও কী-স্ট্রোক দুটোই বাঁচায় এবং প্রায় ক্ষেত্রেই আমাদের কোডবেইসের জটিলতা অনেকাংশে লাঘব করে। 
উদাহরণঃ

```elixir
iex> ~w/i love elixir school/
["i", "love", "elixir", "school"]

iex> ~W/i love elixir school/
["i", "love", "elixir", "school"]
```

আমরা দেখতে পাচ্ছি, ডেলিমিটারের ভেতর স্পেইস নির্ণয় করে লিস্ট বিভাজন করা হয়েছে। 
উপরের উদাহরণ দুটিতে কোন পার্থক্য না থাকলেও 
এস্কেপ সিকুয়েন্স ও ইন্টারপোলেশন ভেদে পার্থক্য হবে।  
যেমনঃ 

```elixir
iex> ~w/i love #{'e'}lixir school/
["i", "love", "elixir", "school"]

iex> ~W/i love #{'e'}lixir school/
["i", "love", "\#{'e'}lixir", "school"]
```

### নাইভ ডেট টাইম 

[নাইভ ডেট টাইম](https://hexdocs.pm/elixir/NaiveDateTime.html) দিয়ে আমরা তাড়াতাড়ি স্ট্রাক্ট তৈরি করতে পারি যা ডেট টাইম কে রিপ্রেজেন্ট করে টাইমজোন **ছাড়া**। 

বেশীরভাগ ক্ষেত্রেই আমরা সরাসরি `NaiveDateTime` স্ট্রাক্ট তৈরি করা থেকে বিরত থাকবো
কিন্তু প্যাটার্ন ম্যাচিং করার সময়ে এটা অনেক কাজে আসে।
যেমনঃ

```elixir
iex> NaiveDateTime.from_iso8601("2015-01-23 23:50:07") == {:ok, ~N[2015-01-23 23:50:07]}
```

### ডেট টাইম 

খুব দ্রুত UTC টাইমজোন সহ `DateTime` স্ট্রাক্ট তৈরিতে [ডেট টাইম](https://hexdocs.pm/elixir/DateTime.html) কাজেে লাগে। 
যেহেতু, এটা UTC টাইমজোনে এবং স্ট্রিং অন্য কোন টাইমজোন কেও নির্দেশ করতে পারে তাই, 
সেকেন্ডের অফসেটকে 
তৃতীয় আইটেম হিসেবে রিটার্ন করা হয়।    

উদাহরণস্বরূপঃ

```elixir
iex> DateTime.from_iso8601("2015-01-23 23:50:07Z") == {:ok, ~U[2015-01-23 23:50:07Z], 0}
iex> DateTime.from_iso8601("2015-01-23 23:50:07-0600") == {:ok, ~U[2015-01-24 05:50:07Z], -21600}
```

## সিজিল তৈরি

এলিক্সিরের অন্যতম উদ্দেশ্য হল অত্যন্ত এক্সটেনসিবল অর্থাৎ বর্ধনশীল ল্যাঙ্গুয়েজ হিসেবে ব্যবহৃত হওয়া। 
কাজেই অবাক হবার কিছুই নেই যে আমরা নিজেরাই কাস্টম সিজিল ব্যবহার করতে পারি। 
এই উদাহরণে আমরা কাস্টম সিজিল ব্যবহার করব যা একটি স্ট্রিংকে আপারকেইসে অর্থাৎ বড় হাতের অক্ষরে রূপান্তর করবে। 
যেহেতু এলিক্সিএরর স্ট্রিং মডিউলে একটি ফাংশন (`String.upcase/1`) রয়েছে এ কাজের জন্য তাই আমরা সেটিই ব্যবহার করব। 

```elixir

iex> defmodule MySigils do
...>   def sigil_p(string, []), do: String.upcase(string)
...> end

iex> import MySigils
nil

iex> ~p/elixir school/
ELIXIR SCHOOL
```

প্রথমে আমরা `MySigils` নামক একটি মডিউল সৃষ্টি করে এর ভেতর `sigil_p` নামে একটি ফাংশন বানিয়েছি। 
যেহেতু `~p` সিজিলের কোন অস্তিত্ব নেই তাই সেটিকে আমরা আমাদের কাজে ব্যবহার করলাম। 
`_p` দিয়ে বুঝানো হয়েছে যে টিল্ডার (~) পর ব্যবহৃত ক্যারেক্টার হবে `p`। 
ফাংশন ডেফিনেশনকে অবশ্যই ২ টি আর্গুমেন্ট নিতে হবে, একটি ইনপুট এবং একটি লিস্ট। 

