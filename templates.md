# Templates

- [Controller Layouts](#controller-layouts)
- [Blade Templating](#blade-templating)
- [Other Blade Control Structures](#other-blade-control-structures)

<a name="controller-layouts"></a>
## Controller Layouts

One method of using templates in Laravel is via controller layouts. By specifying the `layout` property on the controller, the view specified will be created for you and will be the assumed response that should be returned from actions.

<a name="defining-a-layout-on-a-controller"></a>
**Defining A Layout On A Controller**

	class UserController extends BaseController {

		/**
		 * The layout that should be used for responses.
		 */
		protected $layout = 'layouts.master';

		/**
		 * Show the user profile.
		 */
		public function showProfile()
		{
			$this->layout->content = View::make('user.profile');
		}

	}

<a name="blade-templating"></a>
## Blade Templating

Blade is a simple, yet powerful templating engine provided with Laravel. Unlike controller layouts, Blade is driven by _template inheritance_ and _sections_. All Blade templates should use the `.blade.php` extension.

<a name="defining-a-blade-layout"></a>
**Defining A Blade Layout**

	<!-- Stored in app/views/layouts/master.blade.php -->

	<html>
		<body>
			@section('sidebar')
				This is the master sidebar.
			@show

			<div class="container">
				@yield('content')
			</div>
		</body>
	</html>

<a name="using-a-blade-layout"></a>
**Using A Blade Layout**

	@extends('layouts.master')

	@section('sidebar')
		@parent

		<p>This is appended to the master sidebar.</p>
	@stop

	@section('content')
		<p>This is my body content.</p>
	@stop

Note that views which `extend` a Blade layout simply override sections from the layout. Content of the layout can be included in a child view using the `@parent` directive in a section, allowing you to append to the contents of a layout section such as a sidebar or footer.

<a name="other-blade-control-structures"></a>
## Other Blade Control Structures

<a name="echoing-data"></a>
**Echoing Data**

	Hello, {{ $name }}.

	The current UNIX timestamp is {{ time() }}.

To escape the output, you may use the triple curly brace syntax:

	Hello, {{{ $name }}}.

<a name="if-statements"></a>
**If Statements**

	@if (count($records) === 1)
		I have one record!
	@elseif (count($records) > 1)
		I have multiple records!
	@else
		I don't have any records!
	@endif

	@unless (Auth::check())
		You are not signed in.
	@endunless

<a name="loops"></a>
**Loops**

	@for ($i = 0; $i < 10; $i++)
		The current value is {{ $i }}
	@endfor

	@foreach ($users as $user)
		<p>This is user {{ $user->id }}</p>
	@endforeach

	@while (true)
		<p>I'm looping forever.</p>
	@endwhile

<a name="including-sub-views"></a>
**Including Sub-Views**

	@include('view.name')

<a name="displaying-language-lines"></a>
**Displaying Language Lines**

	@lang('language.line')

	@choice('language.line', 1);

<a name="comments"></a>
**Comments**

	{{-- This comment will not be in the rendered HTML --}}
