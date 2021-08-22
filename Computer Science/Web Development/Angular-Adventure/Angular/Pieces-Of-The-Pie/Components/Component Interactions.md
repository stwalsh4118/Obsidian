### Components can interact with each other in various ways.

- Passing data to a child component with an `@Input()` decorator
- Passing data to a parent with an event and `@Output()` decorator
- Directly controlling a child components properties with the `@ViewChild()` decorator

Some specific ways include:
- [[#Input binding]]
- [[#Intercepting input property changes with a setter]]
- [[#Intercepting input property changes with ngOnChanges Lifecycle Hooks lifecycle hook | Intercepting input property changes with ngOnChanges Lifecycle Hook]]
- [[#Parent listening for a child's output event]]
- [[#Parent interacting with child through local variable in template]]
- [[#Accessing a child's properties with the ViewChild decorator]]
- [[#Parent and child communication through a Services Service | Parent and child communication through a Service]]

#### Input binding

We can bind a components property to an attribute in its template by designating the property with an `@Input()` decorator

Additionally you can add an argument in the `@Input()` decorator to specifically designate the html attribute that will set the property you want to bind (the default makes it so the attribute name must be the same as the property name)

Typical Input binding can look like so:

```ts
import { Component, Input } from '@angular/core';

import { Hero } from './hero';

@Component({
  selector: 'app-hero-child',
  template: `
    <h3>{{hero.name}} says:</h3>
    <p>I, {{hero.name}}, am at your service, {{masterName}}.</p>
  `
})
export class HeroChildComponent {
  @Input() hero!: Hero;
  @Input('master') masterName = ''; // tslint:disable-line: no-input-rename
}
```

Above we can see the input properties with the `@Input()` decorator that will be passed down through its html template

```ts
import { Component } from '@angular/core';

import { HEROES } from './hero';

@Component({
  selector: 'app-hero-parent',
  template: `
    <h2>{{master}} controls {{heroes.length}} heroes</h2>
    <app-hero-child *ngFor="let hero of heroes"
      [hero]="hero"
      [master]="master">
    </app-hero-child>
  `
})
export class HeroParentComponent {
  heroes = HEROES;
  master = 'Master';
}
```

Here we can see the attributes `[hero]` and `[master]` being set in the `<app-hero-child>` html element, those will take the data from the parent and pass it into the html element, those will take the data from the parent and pass it into the `<app-hero-child>` component where it can do what it wants with the data, whether it be printing it to the screen in an html element or processing the data or passing it to its own children

##### Intercepting input property changes with a setter

You can add getter and setter methods to your property with an `@Input()` decorator to process the input from the attribute

```ts
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-name-child',
  template: '<h3>"{{name}}"</h3>'
})
export class NameChildComponent {
  @Input()
  get name(): string { return this._name; }
  set name(name: string) {
    this._name = (name && name.trim()) || '<no name set>';
  }
  private _name = '';
}
```

Here we can see that in the setter method we trim the white space from the input or output `<no name set>`

##### Intercepting input property changes with ngOnChanges() [[Lifecycle Hooks| lifecycle hook]]

We can use ngOnChanges() to detect when a property in the component changes. This is useful so we can process the data going into the component or for when we are trying to input async data into the component as we don't know the exact time when that data will get there

```ts
import { Component, Input, OnChanges, SimpleChanges } from '@angular/core';

@Component({
  selector: 'app-version-child',
  template: `
    <h3>Version {{major}}.{{minor}}</h3>
    <h4>Change log:</h4>
    <ul>
      <li *ngFor="let change of changeLog">{{change}}</li>
    </ul>
  `
})
export class VersionChildComponent implements OnChanges {
  @Input() major = 0;
  @Input() minor = 0;
  changeLog: string[] = [];

  ngOnChanges(changes: SimpleChanges) {
    const log: string[] = [];
    for (const propName in changes) {
      const changedProp = changes[propName];
      const to = JSON.stringify(changedProp.currentValue);
      if (changedProp.isFirstChange()) {
        log.push(`Initial value of ${propName} set to ${to}`);
      } else {
        const from = JSON.stringify(changedProp.previousValue);
        log.push(`${propName} changed from ${from} to ${to}`);
      }
    }
    this.changeLog.push(log.join(', '));
  }
}
```

Here we can see that we get the changes in the ngOnChanges() hook and we loop through all of the changes and deal with them accordingly

##### Parent listening for a child's output event

In a child component we can expose and EventEmitter property that can emit a signal whenever the child does anything. The parent can detect that signal and then do something, or take data from the child component.

For child -> parent transmission we can use the `@Output()` decorator  

```ts
import { Component, EventEmitter, Input, Output } from '@angular/core';

@Component({
  selector: 'app-voter',
  template: `
    <h4>{{name}}</h4>
    <button (click)="vote(true)"  [disabled]="didVote">Agree</button>
    <button (click)="vote(false)" [disabled]="didVote">Disagree</button>
  `
})
export class VoterComponent {
  @Input()  name = '';
  @Output() voted = new EventEmitter<boolean>();
  didVote = false;

  vote(agreed: boolean) {
    this.voted.emit(agreed);
    this.didVote = true;
  }
}
```

Here we can see the `voted` property with the `@Output()` decorator that will emit whenever one of the buttons is clicked. It also passes a value with the `emit()` so whatever component receives that signal will also get the value that was passed 

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-vote-taker',
  template: `
    <h2>Should mankind colonize the Universe?</h2>
    <h3>Agree: {{agreed}}, Disagree: {{disagreed}}</h3>
    <app-voter *ngFor="let voter of voters"
      [name]="voter"
      (voted)="onVoted($event)">
    </app-voter>
  `
})
export class VoteTakerComponent {
  agreed = 0;
  disagreed = 0;
  voters = ['Narco', 'Celeritas', 'Bombasto'];

  onVoted(agreed: boolean) {
    agreed ? this.agreed++ : this.disagreed++;
  }
}
```

Here we see that the attribute `(voted)` is the output from the element. Whenever the `(voted)` event is triggered it calls the function `onVoted()` in the parent with the argument `$event`. The `$event` object is the value that was passed in the emit from the child, so in this case it corresponds to the boolean that was passed in the emit.

##### Parent interacting with child through local variable in template

A parent cannot directly access a child's properties or functions by default, but if we set the child with a local variable directive with a # before the variable name we can access *that* components properties via the local variable name. 

```ts
import { Component } from '@angular/core';
import { CountdownTimerComponent } from './countdown-timer.component';

@Component({
  selector: 'app-countdown-parent-lv',
  template: `
  <h3>Countdown to Liftoff (via local variable)</h3>
  <button (click)="timer.start()">Start</button>
  <button (click)="timer.stop()">Stop</button>
  <div class="seconds">{{timer.seconds}}</div>
  <app-countdown-timer #timer></app-countdown-timer>
  `,
  styleUrls: ['../assets/demo.css']
})
export class CountdownLocalVarParentComponent { }
```

Here we can see that the `<app-countdown-timer>` has the `#timer` directive that labels it with the local variable name "timer". We can now access that elements properties within the parent, as we see with the `timer.start()` call on click of the button, etc. This only works directly within the templates, the parent component still **doesn't have direct access to the child.**

##### Accessing a child's properties with the @ViewChild() decorator

We can get a reference to the child element directly within the parent with the `@ViewChild()` decorator. We can either look directly for the element selector (which finds the first one in the template) or we can set an element with a local variable directive like before and refer to the element by that local variable

```ts
import { AfterViewInit, ViewChild } from '@angular/core';
import { Component } from '@angular/core';
import { CountdownTimerComponent } from './countdown-timer.component';

@Component({
  selector: 'app-countdown-parent-vc',
  template: `
  <h3>Countdown to Liftoff (via ViewChild)</h3>
  <button (click)="start()">Start</button>
  <button (click)="stop()">Stop</button>
  <div class="seconds">{{ seconds() }}</div>
  <app-countdown-timer></app-countdown-timer>
  `,
  styleUrls: ['../assets/demo.css']
})
export class CountdownViewChildParentComponent implements AfterViewInit {

  @ViewChild(CountdownTimerComponent)
  private timerComponent!: CountdownTimerComponent;

  seconds() { return 0; }

  ngAfterViewInit() {
    // Redefine `seconds()` to get from the `CountdownTimerComponent.seconds` ...
    // but wait a tick first to avoid one-time devMode
    // unidirectional-data-flow-violation error
    setTimeout(() => this.seconds = () => this.timerComponent.seconds, 0);
  }

  start() { this.timerComponent.start(); }
  stop() { this.timerComponent.stop(); }
}
```

Here we can see that when we invoke the `@ViewChild()` decorator we give it the argument `CountdownTimerComponent` so it will look for that element and save a reference to it in `timerComponent`. We can then access that components properties and functions through that reference like in the `start()` and `stop()` methods where we invoke `this.timerComponent.stop()`, etc.

We can also get a reference through a local variable reference like so:

```ts
<app-countdown-timer #timer></app-countdown-timer>

@ViewChild('timer')
private timerComponent: CountdownTimerComponent;
```

The above gets a reference to the element by using its local variable name "timer"


##### Parent and child communication through a [[Services | Service]]

A parent and child can communicate bi-directionally through a service. ex. a Parent sends a signal to a service and that service sends a signal back to the child, etc. 

```ts
import { Injectable } from '@angular/core';
import { Subject } from 'rxjs';

@Injectable()
export class MissionService {

  // Observable string sources
  private missionAnnouncedSource = new Subject<string>();
  private missionConfirmedSource = new Subject<string>();

  // Observable string streams
  missionAnnounced$ = this.missionAnnouncedSource.asObservable();
  missionConfirmed$ = this.missionConfirmedSource.asObservable();

  // Service message commands
  announceMission(mission: string) {
    this.missionAnnouncedSource.next(mission);
  }

  confirmMission(astronaut: string) {
    this.missionConfirmedSource.next(astronaut);
  }
}
```

Here is the service that we will inject into all of the components that want to communicate together and uses [[Subjects]] to send signals to the components whenever something happens.

```ts
import { Component } from '@angular/core';

import { MissionService } from './mission.service';

@Component({
  selector: 'app-mission-control',
  template: `
    <h2>Mission Control</h2>
    <button (click)="announce()">Announce mission</button>
    <app-astronaut *ngFor="let astronaut of astronauts"
      [astronaut]="astronaut">
    </app-astronaut>
    <h3>History</h3>
    <ul>
      <li *ngFor="let event of history">{{event}}</li>
    </ul>
  `,
  providers: [MissionService]
})
export class MissionControlComponent {
  astronauts = ['Lovell', 'Swigert', 'Haise'];
  history: string[] = [];
  missions = ['Fly to the moon!',
              'Fly to mars!',
              'Fly to Vegas!'];
  nextMission = 0;

  constructor(private missionService: MissionService) {
    missionService.missionConfirmed$.subscribe(
      astronaut => {
        this.history.push(`${astronaut} confirmed the mission`);
      });
  }

  announce() {
    const mission = this.missions[this.nextMission++];
    this.missionService.announceMission(mission);
    this.history.push(`Mission "${mission}" announced`);
    if (this.nextMission >= this.missions.length) { this.nextMission = 0; }
  }
}
```

```ts
import { Component } from '@angular/core';

import { MissionService } from './mission.service';

@Component({
  selector: 'app-mission-control',
  template: `
    <h2>Mission Control</h2>
    <button (click)="announce()">Announce mission</button>
    <app-astronaut *ngFor="let astronaut of astronauts"
      [astronaut]="astronaut">
    </app-astronaut>
    <h3>History</h3>
    <ul>
      <li *ngFor="let event of history">{{event}}</li>
    </ul>
  `,
  providers: [MissionService]
})
export class MissionControlComponent {
  astronauts = ['Lovell', 'Swigert', 'Haise'];
  history: string[] = [];
  missions = ['Fly to the moon!',
              'Fly to mars!',
              'Fly to Vegas!'];
  nextMission = 0;

  constructor(private missionService: MissionService) {
    missionService.missionConfirmed$.subscribe(
      astronaut => {
        this.history.push(`${astronaut} confirmed the mission`);
      });
  }

  announce() {
    const mission = this.missions[this.nextMission++];
    this.missionService.announceMission(mission);
    this.history.push(`Mission "${mission}" announced`);
    if (this.nextMission >= this.missions.length) { this.nextMission = 0; }
  }
}
```

The above both inject the MissionService and subscribe to the Subjects within the service so they know whenever a signal is sent out from the service.
