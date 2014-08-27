#Polymorphic Associations

###So What Are Polymorphic Associations?

![YOLOCITY](http://imoveilive.com/wp-content/uploads/2014/05/mighty_morphin_power_rangers_by_butters101-d73baih.jpg) ######But Better

###Basics:
* When one model can belong to more than one other model through a single association
* Poly - Greek root of pele - meaning to spread
* Morphic - Greek root of morphe - meaning form or shape
* Basically, think about how swineflu can BELONG_TO both humans and swine, without being a different disease #science

###Example:
Models:
```ruby

class Swineflu < ActiveRecord::Base
  belongs_to :death, polymorphic: true
end
 
class Pig < ActiveRecord::Base
  has_many :swineflus, as: :death
end
 
class Human < ActiveRecord::Base
  has_many :swineflus, as: :death
end

```

Migrations:
```ruby

class CreateSwineflus < ActiveRecord::Migration
  def change
    create_table :swineflus do |t|
      t.string  :strain
      t.integer :death_id
      t.string  :death_type

      t.timestamps
    end
  end
end

```
In Short Form:
```ruby

t.references :death, polymorphic: true

```

Let's say that you wanted to see which swineflus @human has been infected by and which host @swineflu has infected:
```ruby

@human.swineflus #=> Will return all swineflus of the human

@swineflu.death #=> Will return an instance of a human or pig which it infected

```


![SoCuteSoDeadly](http://blogs-images.forbes.com/stevensalzberg/files/2011/10/swine-flu1.jpg) 
