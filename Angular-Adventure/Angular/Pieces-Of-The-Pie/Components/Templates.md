The `app.component.html` file is the initialized template for the root when you first create your project

# *.component.html files

An example file can look like so:

```html
<div class="task-container">
	<div class="task-description">{{ timerTask.taskDescription }}</div>
	<cd-timer
		[startTime]="timerTask.taskLength"
		format="intelli"
		(onStart)="autoStop()"
		#timer
		class="task-length"
	></cd-timer>
	<fa-icon [icon]="faTimes" size="lg" (click)="deleteTask()"></fa-icon>
</div>
```