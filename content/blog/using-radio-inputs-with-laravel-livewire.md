---
title: "Using Radio Inputs With Laravel Livewire"
date: 2020-05-11T10:03:45+05:30
draft: false
categories: ["articles"]
keywords:
  - laravel-livewire
  - radio-input
---

Recently I was building a quiz application using Laravel livewire. So in this article I am going to tell u about what circumstances I phased using livewire's in built `wire:model` attribute in radio buttons, for having two way data binding.

> Livewire is a full-stack framework for Laravel that makes building dynamic interfaces simple, without leaving the comfort of Laravel.

<!--more-->

Here I will talk about two approaches that I have tried for using radio buttons with livewire for a better user experience.

## Using wire:model directly in the radio input

So, `wire:model` is similar like v-model (if you are coming from Vue world). It let us have two way data binding for the input component. Whenever my radio input changes it automatically reflects on the backend or whenever my backend data changes it reflects in the ui. In my example:

```php
<?php

namespace App\Http\Livewire\Test;

use Livewire\Component;
use App\Question as TestQuestion;

class Question extends Component
{
    public $answer, $question;

    public function mount()
    {
        $this->question = TestQuestion::with('options', 'answer')->inRandomOrder()->first();
    }

    public function render()
    {
        return view('livewire.test.question');
    }
}
```

And the blade file looks like:

```php
<ul class="text-gray-700 text-base sm:text-lg">
    @foreach ($question->options as $index => $option)
    <li class="{{$loop->last ? '' : 'mb-4'}}">
        <label for="{{$index}}" class="items-center clearfix relative inline-flex border-2 p-3 rounded-lg">
        <input id="{{$index}}" type="radio" wire:model="answer" class="check-custom" name="answer[{{$index}}]" value="{{$option->id}}">
        <span class="check-toggle flex-shrink-0 large"></span>
        <span class="ml-2">
            <div class="flex">
            <div class="pl-2">
                <h5 class="text-gray-700">{{$option->correct}}{{$option->option_text}}</h5>
            </div>
            </div>
        </span>
        </label>
    </li>
    @endforeach
</ul>
```

> So, the above solution works great and the data changes accordingly. But, for users using a slower internet connection might get affected in this approach. Since, livewire always triggers a network call whenever the component's property changes, so the radio input will not toggle until the network call gets succeded which is not a good ui experience. We don't want our users to wait for every single toggle of the radio button.

So it hits my mind, and I was thinking like what to do now. So, if we could listen to the radio toggle in the frontend itself, we could overcome this sceario. In the next, `alpinejs` comes to my mind. Alpine can handle this type of scenario so easily, and livewire have good api for using alpine's data with the livewire properties.

## Using Alpine's x-model in the radio input

I assume you have already installed `alpine.js` in your project. Our blade file looks like:

```php
<div class="container px-4 md:px-0 max-w-4xl mx-auto -mt-40 md:-mt-32" x-data="{'answer': ''}">
    <div class="flex justify-between items-center">
        <button @click="@this.call('clickNext', `${answer}`);
                        answer = ''"
            role="button"
            class="w-1/3 p-4 text-center focus:outline-none rounded-lg bg-orange-400 hover:bg-orange-600 text-white text-center"
            x-bind:class="{ 'cursor-not-allowed opacity-50' : !answer }"
            x-bind:disabled="!answer"
        >
            <span>Next</span>
            <span class="hidden sm:inline-block"> &rarr;</span>
        </button>
    </div>
    <ul class="text-gray-700 text-base sm:text-lg">
        @foreach ($question->options as $index => $option)
        <li class="{{$loop->last ? '' : 'mb-4'}}">
            <label for="{{$index}}" class="items-center clearfix relative inline-flex border-2 p-3 rounded-lg">
            <input id="{{$index}}" type="radio" x-model="answer" class="check-custom" name="answer[{{$index}}]" value="{{$option->id}}">
            <span class="check-toggle flex-shrink-0 large"></span>
            <span class="ml-2">
                <div class="flex">
                <div class="pl-2">
                    <h5 class="text-gray-700">{{$option->correct}}{{$option->option_text}}</h5>
                </div>
                </div>
            </span>
            </label>
        </li>
        @endforeach
    </ul>
</div>
```

In the above you could see, I have using `x-data` to define our answer property. At first it is empty. Then I used `x-model="answer"` attribute to listen for radio input's data changes in alpine. Then on the click of a button (form submit button), I am calling the `clickNext` function in the component, and send the answer as a payload from the `x-data`. And in the backend I am setting the `answer` attribute with our payload. Our `clickNext` method looks like:

```php
<?php

namespace App\Http\Livewire\Test;

use Livewire\Component;
use App\Question as TestQuestion;

class Question extends Component
{
    public $answer, $question;

    public function clickNext($answerId, Test $test)
        {
            $this->answer = $answerId;
            // do other stuff
        }
}
```

So, from the above solution we have handled the problem we are facing previously. So, if you have any solution rather than this please share and comment on the same in twitter.
