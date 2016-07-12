# Sqlserver::Sequence

[![Code Climate](https://codeclimate.com/github/zacharywelch/sqlserver-sequence/badges/gpa.svg)](https://codeclimate.com/github/zacharywelch/sqlserver-sequence)
[![Issue Count](https://codeclimate.com/github/zacharywelch/sqlserver-sequence/badges/issue_count.svg)](https://codeclimate.com/github/zacharywelch/sqlserver-sequence)
[![Gem Version](https://badge.fury.io/rb/sqlserver-sequence.svg)](https://badge.fury.io/rb/sqlserver-sequence)

A Rails plugin for SQL Server sequences.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'sqlserver-sequence'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install sqlserver-sequence

## Usage

Specify the attribute you'd like to sequence

```ruby
class Supplier < ActiveRecord::Base
  sequence :number
end
```

Now when you create a record `number` will be assigned the next value for that sequence.

    $ supplier = Supplier.create
    $ supplier.number
    > 10 # generated by sequence named 'number'

    $ other_supplier = Supplier.create
    $ other_supplier.number
    > 11

You can also define multiple sequences

```ruby
class Supplier < ActiveRecord::Base
  sequence :number
  sequence :purchase_order
end
```

### Options

###### name

By default `sqlserver-sequence` will look for a row in `sys.sequences` with the same `sequence_name` as the attribute. You can use the `name` option if the sequence is named differently.

```ruby
class Supplier < ActiveRecord::Base
  sequence :number, name: 'next_supplier_number'
end
```
    $ supplier = Supplier.create
    $ supplier.number 
    > 10 # generated by sequence named 'next_supplier_number'

###### prefix

You can use the `prefix` option to prepend every sequence value with a string.

```ruby
class Supplier < ActiveRecord::Base
  sequence :number, prefix: 'S-'
end
```
    $ supplier = Supplier.create
    $ supplier.number
    > S-10

###### format

Pass a lambda to the `format` option if you need more control over the assigned sequence value, for example to assign a fixed length string.

```ruby
class Supplier < ActiveRecord::Base
  sequence :number, prefix: 'S-',
                    format: lambda { |num| num.rjust(10, 0) }
end
```
    $ supplier = Supplier.create
    $ supplier.number
    > S-0000000010

## Testing with SQLite

If your test suite uses SQLite you'll need to turn off sequence generation or provide a stub for `next_sequence_value`.

```ruby
before do
  allow_any_instance_of(Supplier).to receive(:next_sequence_value).and_return('10')
end
```

## Contributing

1. Fork it ( https://github.com/zacharywelch/sqlserver-sequence/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request

See the [Running Tests](RUNNING_TESTS.md) guide for details on how to run the test suite on SQL Server.
