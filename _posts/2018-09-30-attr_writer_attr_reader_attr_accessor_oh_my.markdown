---
layout: post
title:      "Attr_writer, attr_reader, attr_accessor, oh my!"
date:       2018-10-01 01:10:06 +0000
permalink:  attr_writer_attr_reader_attr_accessor_oh_my
---


What are these attribute beasts? Moreover, Ruby's "has many," "belongs to," and "has many through" methods makes me feel like Dorothy deep inside dark, scary woods. Luckily, I found this [great video tutorial](https://instruction.learn.co/student/video_lectures#/340) to protect and save me. 

Regretably, I didn't come up with a Wizard of Oz example to post here, but instead built some object relationships based on real estate. First, I'll post the code in full, then break it down for inspection.


    class House
       attr_accessor :neighborhood
       attr_reader :realtor, :buyer

       def initialize(neighborhood)
          @neighborhood = neighborhood
       end

       def realtor=(realtor)
          @realtor = realtor
          realtor.add_house(self) if !realtor.houses.include?(self)
       end

       def buyer=(buyer)
         @buyer = buyer 
         buyer.add_house(self) if !buyer.houses.include?(self)
       end
    end


    class Realtor 
       attr_accessor :neighborhood
       attr_reader :houses

       def initialize(name) 
         @name = name 
         @houses = []
       end

       def add_house(house)
         @houses << house
         house.realtor = self unless house.realtor != self
       end
    end


    class Buyer 
        attr_reader :houses, :buyer_name 

        def initialize(buyer_name)
          @buyer_name = buyer_name 
          @houses = []
        end 

        def add_house(house)
          @houses << house 
           house.buyer = self if house.buyer != self
        end
    end 


Now, let's go over some details. 

First, a realtor will have many prospective buyers through the houses he's selling. In other words, the House class is the bridge between the Realtor class and the Buyer class.

Second, a realtor will have many houses he's selling (let's hope), so we must build an array to store each instance of the House class. As for the buyer, he will have at least one house he's interested in looking at with his realtor, so we need to store those instances in an array, as well. I've chosen to represent this as @houses = [] .  I've also chosen to attach the attr_reader to :houses to protect the array from potential manipulation by someone else's code. 

To review, attr_reader is like a window that allows the user to look at data without interacting with it. On the other hand, attr_writer allows the user to set or overwrite data. However, the user can't see the data. That's why attr_accessor exists, to combine attr_reader and attr_writer together for dual functionality.

Back to the real estate example, I used the "if" and "unless" keywords interchangeably to build code that protects against infinite relationship loops. For example, in the "def add_house(house)" method, I only want to add a house into the @houses array if the house doesn't yet exist. 

Now, for the fun part, let's test the code: 

    barry = Realtor.new("Barry")
    => #<Realtor:0x000056258db49ea8 @name="Barry", @houses=[]>
  
    lake = House.new("Lake of the Isles")
    => #<House:0x000056258db49430 @neighborhood="Lake of the Isles">
  
    edina = House.new("Edina")
    => #<House:0x000056258db48a80 @neighborhood="Edina">
		
    johnson = Buyer.new("The Johnsons")
    => #<Buyer:0x000056258db48148 @buyer_name="The Johnsons", @houses=[]>
		
    anderson = Buyer.new("The Andersons")
    => #<Buyer:0x000056258db3f7a0 @buyer_name="The Andersons", @houses=[]>
   
	 lake.realtor = barry
    => #<Realtor:0x000056258db49ea8 @name="Barry", @houses=[#<House:0x000056258db49430 @neighborhood="Lake of the Isles", @realtor=#<Realtor:0x000056258db49ea8 ...>>]>
		
    edina.realtor = barry
    => #<Realtor:0x000056258db49ea8 @name="Barry", @houses=[#<House:0x000056258db49430   @neighborhood="Lake of the Isles", @realtor=#<Realtor:0x000056258db49ea8 ...>>, #<House:0x000056258db48a80 @neighborhood="Edina", @realtor=#<Realtor:0x000056258db49ea8 ...>>]>
   
	 lake.buyer = johnson
    => #<Buyer:0x000056258db48148 @buyer_name="The Johnsons", @houses=[#<House:0x000056258db49430 @neighborhood="Lake of the Isles", @realtor=#<Realtor:0x000056258db49ea8 @name="Barry", @houses=[#<House:0x000056258db49430 ...>, #<House:0x000056258db48a80 @neighborhood="Edina", @realtor=#<Realtor:0x000056258db49ea8 ...>>]>, @buyer=#<Buyer:0x000056258db48148 ...>>]>
		
    edina.buyer = anderson
    => #<Buyer:0x000056258db3f7a0 @buyer_name="The Andersons", @houses=[#<House:0x000056258db48a80 @neighborhood="Edina", @realtor=#<Realtor:0x000056258db49ea8 @name="Barry", @houses=[#<House:0x000056258db49430 @neighborhood="Lake of the Isles", @realtor=#<Realtor:0x000056258db49ea8 ...>, @buyer=#<Buyer:0x000056258db48148 @buyer_name="The Johnsons", @houses=[#<House:0x000056258db49430 ...>]>>, #<House:0x000056258db48a80 ...>]>, @buyer=#<Buyer:0x000056258db3f7a0 ...>>]>
		
		
Whew! How exciting is that? Feels like magic.

Anyway, there you have it -- a simple, functioning object relationship story. 


