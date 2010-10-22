Prawn Rails
==========

Prawn Rails provides a simple way of creating PDF views in Rails 3 using the prawn library.

To use Prawn Rails simply add the line

    gem 'prawn_rails'
    
to your Gemfile and then run

    bundle install
    
That's it!  You can now create views named
    
    [action].pdf.prawn

which will be used whenever the user requests a page with a 'pdf' extension

Usage
-----

prawn_rails is designed to provide only a very thin wrapper around Prawn itself.  A prawn_rails view should consist of only a call to the function prawn_document and a block.  This will create an instance of Prawn::Document and yield it to the block.
For a simple pdf view try:

simple.pdf.prawn

    prawn_document() do |pdf|
      pdf.text "Hello World"
    end

This will create a simple PDF with only the text Hello World.

Like normal Rails views, instance variables assigned in the controller are made available in the view.  For example:

home_controller.rb

    class HomeController < ApplicationController
      def index
        @people=['Jane','John','Jack']
      end
    end
    
index.pdf.prawn

    prawn_document(:page_layout => :landscape) do |pdf|
      @people.each {|person| pdf.text person}
    end
    
This will produce a pdf with Jane, John, and Jack all written on seperate lines.

Notice we passed a hash into prawn_document.  Any parameters placed in this hash will be passed to the constructor of Prawn::Document, with one exception:

override.pdf.prawn
    
    prawn_document({:renderer => ApplicationHelper::Foo})
    
The renderer option allows you to override the class to be used for rendering with a subclass of Prawn::Document.  So for the view above you could have an application helper file that looks like:

application_helper.rb

    module ApplicationHelper
      class Foo < Prawn::Document
        def initialize
          super
          text "Foo"
          text "Bar"
        end
      end
    end

This would generate a canned report with just the lines Foo and Bar.

Gotchas
-------

The one major gotcha at this point is that layouts do not work.  Do not attempt to make an app/views/layouts/application.pdf.prawn.  All your pdf views will quit.  This is something I hope to fix in a later release.  In the meantime I recommend using custom classes like the one above to achieve a similair effect.
    
Examples
--------

For examples see: [http://github.com/Volundr/prawn_rails_demo](http://github.com/Volundr/prawn_rails_demo)


Copyright (c) 2010 Walton Hoops, released under the MIT license