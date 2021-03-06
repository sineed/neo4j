Property
========






.. toctree::
   :maxdepth: 3
   :titlesonly:


   Property/UndefinedPropertyError

   Property/MultiparameterAssignmentError

   

   

   

   

   

   

   

   

   

   

   

   

   

   

   Property/ClassMethods




Constants
---------



  * DATE_KEY_REGEX

  * DEPRECATED_OBJECT_METHODS



Files
-----



  * `lib/neo4j/shared/property.rb:2 <https://github.com/neo4jrb/neo4j/blob/master/lib/neo4j/shared/property.rb#L2>`_





Methods
-------



.. _`Neo4j/Shared/Property#==`:

**#==**
  Performs equality checking on the result of attributes and its type.

  .. code-block:: ruby

     def ==(other)
       return false unless other.instance_of? self.class
       attributes == other.attributes
     end



.. _`Neo4j/Shared/Property#[]`:

**#[]**
  

  .. code-block:: ruby

     def read_attribute(name)
       respond_to?(name) ? send(name) : nil
     end



.. _`Neo4j/Shared/Property#[]=`:

**#[]=**
  Write a single attribute to the model's attribute hash.

  .. code-block:: ruby

     def write_attribute(name, value)
       if respond_to? "#{name}="
         send "#{name}=", value
       else
         fail Neo4j::UnknownAttributeError, "unknown attribute: #{name}"
       end
     end



.. _`Neo4j/Shared/Property#_persisted_obj`:

**#_persisted_obj**
  Returns the value of attribute _persisted_obj

  .. code-block:: ruby

     def _persisted_obj
       @_persisted_obj
     end



.. _`Neo4j/Shared/Property#assign_attributes`:

**#assign_attributes**
  Mass update a model's attributes

  .. code-block:: ruby

     def assign_attributes(new_attributes = nil)
       return unless new_attributes.present?
       new_attributes.each do |name, value|
         writer = :"#{name}="
         send(writer, value) if respond_to?(writer)
       end
     end



.. _`Neo4j/Shared/Property#attribute_before_type_cast`:

**#attribute_before_type_cast**
  Read the raw attribute value

  .. code-block:: ruby

     def attribute_before_type_cast(name)
       @attributes ||= {}
       @attributes[name.to_s]
     end



.. _`Neo4j/Shared/Property#attributes`:

**#attributes**
  Returns a Hash of all attributes

  .. code-block:: ruby

     def attributes
       attributes_map { |name| send name }
     end



.. _`Neo4j/Shared/Property#attributes=`:

**#attributes=**
  Mass update a model's attributes

  .. code-block:: ruby

     def attributes=(new_attributes)
       assign_attributes(new_attributes)
     end



.. _`Neo4j/Shared/Property#initialize`:

**#initialize**
  

  .. code-block:: ruby

     def initialize(attributes = nil)
       attributes = process_attributes(attributes)
       @relationship_props = self.class.extract_association_attributes!(attributes)
       modded_attributes = inject_defaults!(attributes)
       validate_attributes!(modded_attributes)
       writer_method_props = extract_writer_methods!(modded_attributes)
       send_props(writer_method_props)
       @_persisted_obj = nil
     end



.. _`Neo4j/Shared/Property#inject_defaults!`:

**#inject_defaults!**
  

  .. code-block:: ruby

     def inject_defaults!(starting_props)
       return starting_props if self.class.declared_properties.declared_property_defaults.empty?
       self.class.declared_properties.inject_defaults!(self, starting_props || {})
     end



.. _`Neo4j/Shared/Property#inspect`:

**#inspect**
  

  .. code-block:: ruby

     def inspect
       attribute_descriptions = inspect_attributes.map do |key, value|
         "#{Neo4j::ANSI::CYAN}#{key}: #{Neo4j::ANSI::CLEAR}#{value.inspect}"
       end.join(', ')
     
       separator = ' ' unless attribute_descriptions.empty?
       "#<#{Neo4j::ANSI::YELLOW}#{self.class.name}#{Neo4j::ANSI::CLEAR}#{separator}#{attribute_descriptions}>"
     end



.. _`Neo4j/Shared/Property#read_attribute`:

**#read_attribute**
  

  .. code-block:: ruby

     def read_attribute(name)
       respond_to?(name) ? send(name) : nil
     end



.. _`Neo4j/Shared/Property#reload_properties!`:

**#reload_properties!**
  

  .. code-block:: ruby

     def reload_properties!(properties)
       @attributes = nil
       convert_and_assign_attributes(properties)
     end



.. _`Neo4j/Shared/Property#send_props`:

**#send_props**
  

  .. code-block:: ruby

     def send_props(hash)
       return hash if hash.blank?
       hash.each { |key, value| send("#{key}=", value) }
     end



.. _`Neo4j/Shared/Property#write_attribute`:

**#write_attribute**
  Write a single attribute to the model's attribute hash.

  .. code-block:: ruby

     def write_attribute(name, value)
       if respond_to? "#{name}="
         send "#{name}=", value
       else
         fail Neo4j::UnknownAttributeError, "unknown attribute: #{name}"
       end
     end





