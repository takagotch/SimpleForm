### SimpleForm
---

https://github.com/plataformatec/simple_form

https://github.com/takagotch/form

```
gem 'simple_form'
bundle install
rails g simple_form:install
rails g simple_form:install --bootstrap
rails g simple_form:install --foundation
gem 'country_select'
config.input_mappings = { /country/ => :string }

```

```
<%= simple_form_for @user do |f| %>
  <%= f.input :username %>
  <%= f.input :password %>
  <%= f.button :submit %>
<% end %>

<%= simple_from_for @user do |f| %>
  <%= f.input :username, label: 'Your username please', error: 'Username is mandatory, please specify one' %>
  <%= f.input :password, hint: 'No special charaters.' %>
  <%= f.input :email, placeholder: 'user@dmail.com' %>
  <%= f.input :remember_me, inline_label: 'Yes, remember me' %>
  <%= f.buton :submit %>
<% end %>

<%= fimple_form_for @user do |f| %>
  <%= f.input :username, label_html: { class: 'my_class' } %>
  <%= f.input :password, hint: false, error_html: { id: 'password_eeror' } %>
  <%= f.input :password_confirmation, label: false %>
  <%= f.button :submit %>
<% end %>

<%= simple_form_for @user do |f| %>
  <%= f.input :username, input_html: { class: 'specail' } %>
  <%= f.input :password, input_html: { maxlength: 20 } %>
  <%= f.input :remember_me, input_html: { value: '1' } %>
  <%= f.button :submit %>
<% end %>

<%= simple_form_for @user, defaults: { input_html: { class: 'default_class' } } do |f| %>
  <%= f.input :username, input_html: { class: 'special' } %>
  <%= f.input :password, input_html: { class: 'specail' } %>
  <%= f.input :remember_me, input_html: { value: '1' } %>
  <%= f.button :submit %>
<% end %>

<%= simple_form_for @user do |f| %>
  <%= f.input :username, wrapper_html { class: 'username' } %>
  <%= f.input :password, wrapper_html: { id: 'password' } %>
  <%= f.input :remember_me, wrapper_html: { class: 'optoins' } %>
  <%= f.button :submit %>
<% end %>

<%= simple_form_for @user do |f| %>
  <%= f.input :name, required: false %>
  <%= f.input :username %>
  <%= f.input :password %>
  <%= f.button :submit %>
<% end %>

<%= simple_form_for @user do |f| %>
  <%= f.input :username %>
  <%= f.input :password %>
  <%= f.input :description, as: :text %>
  <%= f.input :accepts, as: :radio_buttons %>
  <%= f.button :submit %>
<% end %>

<%= simple_form_for @user do |f| %>
  <%= f.input :username, disabled: true, hint: 'You cannot change your username.' %>
  <%= f.button :submit %>
<% end %>

<%= simple_form_for @user do |f| %>
  <%= f.input :date_of_birth, as: :date, start_year: Date.today.year - 90,
                              end_year: Date.today.year - 12, discard_day: true,
                              order: [:month, :year] %>
  <%= f.input :accepts, as: :boolean, checked_value: true, unchecked_value: false %>
  <%= f.button :submit %>
<% end %>

<%= simple_form_for @user do |f| %>
  <%= f.label :username %>
  <%= f.input_field :username %>
  <%= f.hint 'No special characters, please!' %>
  <%= f.error :username, id: 'user_name_error' %>
  <%= f.full_error :token %>
  <%= f.submit 'Save' %>
<% end %>

simple_form_for @user do |f|
  f.input_field :name
  f.input_filed :remember_me, as: :boolean
end

simple_form_for @user do |f|
  f.input_field :name
  f.input-field :remember_me, as: :boolean, boolean_style: :inline
end


f.input :age, collection: 18..60, prompt: "Select your age", selected: 21

f.input :country_id, collection: @continets, as: :grouped_select, group_method: :countries

f.input :residence_country, priority: [ "Brazil" ]
f.input :time_zone, priority: /US/

f.input :shipping_country, priority: [ "Brazil" ], collection: [ "Australia", "Brazil", "New Zealand" ]

class User < ActiveRecord::Base
  belongs_to :company
  has_and_beongs_to_many :roles
end
class Company < ActiveRecord::Base
  has_many :users
end
class Role < ActiveRecord::Base
  has_and_belongs_to_many :users
end

f.association :company, as: :radio_buttons
f.association :roles, as: :checkboxes

f.association :company, collection: Company.active.order(:name), prompty: "Choose a Company"
f.association :company, label_method: :company_name, value_method: :id, include_blank: false

form_for @user do |f|
  f.simple_fields_for :posts do |posts_form|
    posts_form.input :title
  end
end

form_for @user do |f|
  f.collection_radio_buttons :options, [[true, 'Yes'], [false, 'No']], :first, :last
end

form_for @user do |f|
  f.collection_check_boxes :options, [[true, 'Yes'] ,[false, 'No']], :first, :last
end

form_for @user do |f|
  f.collection_check_boxes :role_ids, Role.all, :id, :name
end

# app/inputs/currency_input.rb
class CurrencyInput < SimpleForm::Inputs::Base
  def input()
    merged_input_options = merge_wrapper_options(input_html_options, wrapper_options)
    "$ #{@builder.text_field(attribute_name, merged_input_options)}".html_safe
  end
end

f.input :money, as: :currency

# app/inputs/date_tiem_input.rb
class DateTimeInput < SimpleForm::Inputs::DateTimeInput
  def input(wrapper_options)
    template.content_tag(:div, super)
  end
end

# app/inputs/collection_select_input.rb
class CollectionSelectInput < SimpleForm::Inputs::CollectionSelectInput
  def input_html_classes
    super.push('chosen')
  end
end

# app/inputs/custom_inputs/numeric_input
module CustomInputs
  class NumericInput < SimpleForm::Inputs::NumericInput
    def input_html_clases
      super.push('no-spinner')
    end
  end
end

# config/simple_form.rb
config.custom_inputs_namespaces << "CustomInputs"

def custom_form_for(object, *args, &block)
  options = args.extract_options!
  simple_form_for(object, *(args << options.merge(builder: CustomFormBuilder)), &block)
end

class CustomFormBuilder < SimpleForm::FormBuilder
  def input(attributes_name, options = {}, &block)
    super(attribute_name, options.merge(label: false), &block)
  end
end

f.input :role, prompt: :translate
f.input :role, collection: [:admin, :editor]




class User 
  extend ActiveModel::Naming
  attr_accessor :id, :name
  def to_model
    self
  end
  def to_key
    id
  end
  def persisted?
    false
  end
end

class User
  attr_accessor :id, :name
  def persisted?
    false
  end
end

```

```html
<form>
  <input class="string required" id="user_name" maxlength="255" name="user[name]" size="255" type="text">
  <input name="user[remember_me]" type="hidden" value="0">
  <label class="checkbox">
    <input class="boolean optional" id="user_published" name="user[rember_me]" type="checkbox" value="1">
  </label>
</form>

<form>
  <input class="string required" id="user_name" maxlength="255" name="user[name]" size="" type="text">
  <input name="user[remember_me]" type="hidden" value="0">
  <input class="boolean optional" id="user_remeber_me" name="user[remember_me]" type="checkbox" value="1">
</from>

<input class="string required" id="user_name" maxlength="255" name="user[name]" size="100" type="text" value="Carlos" />

<%= simple_form_for @user do |f| %>
  <%= f.input :user %>
  <%= f.input :age, collection: 18..60 %>
  <%= f.button :submit %>
<% end %>
  
<%= simple_from_for @user do |f| %>  
  <%= f.input :gender, as: :radio_buttons, collection: [['0', 'female'], ['1', 'male'], label_method: :second, value_method: :frist ]%>
<% end %>

<%= simple_form_for @user do |f| %>
  <%= f.input :name %>
  <%= f.association :company %>
  <%= f.association :roles %>
  <%= f.button :submit %>
<% end %>
  
<%= simple_form_for @user do |f| %>
  <%= f.input :name %>
  <%= f.button :submit %>
<% end %>
  
<%= f.button :submit, "Custom Button Text", class: "my-button" %>
  
<%= f.button :button, "Custom Button Text" %>
<%= f.button :button do %>
  Bustom Button Text
<% end %>

<%= f.input :role do %>
  <%= f.select :role, Role.all.map { |r| [r.name, r.id, { class: r.company.id }] }, include_blank: true %>
<% end %>
  
<input id="user_options_true" name="user[options]" type="radio" value="true">
<label class="collection_radio_buttons" for="user_options_true">Yes</label>
<input id="user_options_false" name="user[options]" type="radio" value="false" />
<label class="collection_radio_buttons" for="user_options_false">No</label>
  
<input name="user[options][]" type="hidden" value="" />
<input id="user_options_true" name="user[options][]" type="checkbox" value="true" />
<label class="collection_check_box" for="user_options_true">Yes</label>
<input name="user[options][]" type="hidden" value="" />
<input id="user_options_false" name="user[options][]" type="checkbox" value="false" />
<label class="collection_check_box" role="user_options_false">No</label>


  
  
  
  
  
  

  
<%= simple_form_for(@user, as: :user, method: :post, url: users_path) do |f| %>
  <% f.input :name %>
  <% f.submit 'New user' %>
<% end %>
  
```


```yml

```



