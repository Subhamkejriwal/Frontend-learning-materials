##-------------------------  Backend API integration to UI step by step ---------------------------------------##

1.) create a service.ts class where you have to make an API call :-

Example :-
              constructor(private http: HttpClient) {  }

              submitContactusForm(data: any): Observable<any>{
              const url = 'http://localhost:8091/api/aayojan/v1/user/contact-us';
              return this.http.post(url, data,  { responseType: 'text' });
              }

Here you will create the POST,GET etc method that you are making the API call of.


2.) After this in the module.ts page of the component/page go and do like this :- 

Example :- 
 
               @NgModule({
               imports: [
               CommonModule,
               FormsModule,
               IonicModule,
               ReactiveFormsModule,
               ContactusPageRoutingModule,
               HttpClientModule
               ],
               providers: [ProductService],
               declarations: [ContactusPage],
               bootstrap: [AppComponent]
               })
               export class ContactusPageModule {}

Here in providers call the service page that you created previously.

3.) After this go to component/page .ts page of component/page and do like this :-

Example :-

          export class ContactusPage{
               senderName!: string;
               senderEmail!: string;
               senderMobile!: string;
               subject!: string;
               message!: string;

Here first declare the fields that you need to get data of or those you declared. Then 


         constructor(private productService: ProductService, private fb :FormBuilder)  {  }


  onSubmit(){
    const contactUsData = {
      senderName: this.senderName,
      senderEmail: this.senderEmail,
      senderMobile: this.senderMobile,
      subject: this. subject,
      message: this.message
    };

    this.productService.submitContactusForm(contactUsData).subscribe(
      response => {
        console.log(contactUsData);
        alert('Thank you for contacting us! We will be in touch with you shortly.');
      },
      error =>{
        console.error(error);
        alert('Oops! Something went wrong. Please try again later.');
      }
    )
  }
}

##--------------------------------Forms Operation in HTML using which you can edit/add/delete columns and also remove the input fields after the data is entered -------------------##


1.) .HTML page where you are gonna declare your input fields :- 

<div class="p-5">
  <div class="card">
    <div class="card-body">
      <h5 class="mb-3">HOST NEW EVENT : </h5>
      <div class="dropdown d-flex">
        <h5 class="me-3">EVENT TYPE : </h5>
        <select class="form-select event-Type-Width" aria-label="Default select example">
          <option selected>Event Type</option>
          <option value="1">Marriage</option>
          <option value="2">Birthday</option>
          <option value="3">I Am Rich Thats why</option>
        </select>
      </div>


      <div class="mt-3 d-flex">
        <h5 class="me-3">EVENT DATE : </h5>
        <form class="row row-cols-sm-auto">
          <div class="col-12">
            <div class="dp-hidden position-absolute">
              <div class="input-group">
                <input name="datepicker" class="form-control" ngbDatepicker #datepicker="ngbDatepicker"
                  [autoClose]="'outside'" (dateSelect)="onDateSelection($event)" [displayMonths]="2" [dayTemplate]="t"
                  outsideDays="hidden" [startDate]="fromDate!" tabindex="-1" [minDate]="minDate" />
                <ng-template #t let-date let-focused="focused">
                  <span class="custom-day" [class.focused]="focused" [class.range]="isRange(date)"
                    [class.faded]="isHovered(date) || isInside(date)" (mouseenter)="hoveredDate = date"
                    (mouseleave)="hoveredDate = null">
                    {{ date.day }}
                  </span>
                </ng-template>
              </div>
            </div>
            <div class="input-group">
              <input #dpFromDate class="form-control" placeholder="yyyy-mm-dd" name="dpFromDate"
                [value]="formatter.format(fromDate)" (input)="fromDate = validateInput(fromDate, dpFromDate.value)" />
              <button class="btn btn-outline-secondary btn-sm bi bi-calendar3" (click)="datepicker.toggle()"
                type="button"></button>
            </div>
          </div>
          <div class="col-12">
            <div class="input-group">
              <input #dpToDate class="form-control" placeholder="yyyy-mm-dd" name="dpToDate"
                [value]="formatter.format(toDate)" (input)="toDate = validateInput(toDate, dpToDate.value)" />
              <button class="btn btn-outline-secondary bi bi-calendar3" (click)="datepicker.toggle()"
                type="button"></button>
            </div>
          </div>
        </form>
      </div>

      
      <div class="mt-4 mb-3 scrolltable">
        <h5>OCCASIONS LIST:</h5>
        <div>
          <button class="btn btn-primary" (click)="addRow()">Add Functions</button>
        </div>

        <div class="scrolltable mt-3">
          <table class="table table-bordered">
            <thead>
              <tr>
                <th scope="col">Occasions</th>
                <th scope="col">Date</th>
                <th scope="col">Time</th>
                <th scope="col">Guest Type</th>
                <th scope="col"></th>
                <th scope="col"></th>
                <th scope="col"></th>
              </tr>
            </thead>
            <tbody>
              <tr *ngFor="let row of rows; let i = index;">
                <td>
                  <input type="tel" class="form-control" [(ngModel)]="row.occasions">
                </td>
                <td>
                  <form class="row row-cols-sm-auto">
                    <div class="col-12">
                      <div class="input-group">
                        <input type = "tel" class="form-control" placeholder="yyyy-mm-dd" name="dp" [(ngModel)]="row.date"
                          ngbDatepicker #d="ngbDatepicker" />
                        <button class="btn btn-outline-secondary bi bi-calendar3" (click)="d.toggle()"
                          type="button"></button>
                      </div>
                    </div>
                  </form>
                </td>
                <td>
                  <ngb-timepicker [(ngModel)]="row.time" [meridian]="meridian"></ngb-timepicker>
                </td>
                <td>
                  <input type="text" class="form-control" [(ngModel)]="row.guesttype">
                </td>
                <td>
                  <button class="btn btn-success" (click)="saveData(row)">
                    <i class="bi bi-person-add"></i>
                  </button>
                </td>
              </tr>
            </tbody>
            <tbody>
              <tr *ngFor="let function of collection; let i = index;">
                <td>{{function.occasions}}</td>
                <td>{{function.date.year}}-{{function.date.month}}-{{function.date.day}}</td>
                <td>{{function.time.hour}}:{{function.time.minute}}</td>
                <td>{{function.guesttype}}</td>
                <td>
                  <button class="btn btn-primary" (click)="editRow(i, function)">
                    <ion-icon name="create-outline"></ion-icon>
                  </button>
                </td>
                <td>
                  <button class="btn btn-danger" (click)="deleteRow(i)">
                    <i class="bi bi-trash-fill"></i>
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</div>

In this there are total of 3 part. 1st part contain the whole starting of html page and normal stuffs. 2nd part is the date selection you can use date picker instead of hardcoding the calendar just use the datepicker/timepicker that would be easy. 3rd part is the main thing here the table is made in which 4 input fields are there 2 of which are normal string but the other two are date picker and time picker. Here i tried to do like when after filling the input fields when the user press add the data will be shown in the next column and the input fields will become empty. And when the edit button is pressed the data goes back to the input fields to get editted. I have created inClick event for everything 

2.) component.ts code for the events and datepicker/timepicker and other stuffs some of which i dont even know but they are of use :- 

import { Component } from '@angular/core';
import { NgbCalendar, NgbDate, NgbDateParserFormatter, NgbDateStruct } from '@ng-bootstrap/ng-bootstrap';

interface RowData {
	date: NgbDateStruct;
	time: { hour: number; minute: number };
	occasions: string;
	guesttype: string;
}

@Component({
	selector: 'app-event-schedule',
	templateUrl: './event-schedule.component.html',
	styleUrls: ['./event-schedule.component.scss'],
})
export class EventScheduleComponent {
	rows: any[] = [];

	collection: RowData[] = [];

	minDate: NgbDateStruct = {
		year: new Date().getFullYear(),
		month: new Date().getMonth() + 1,
		day: new Date().getDate(),
	};

	model: NgbDateStruct;
	time = { hour: 12, minute: 0 };
	meridian = true;
	modalReference: any;

	toggleMeridian() {
		this.meridian = !this.meridian;
	}

	addRow() {
		this.rows.push({
			occasions: '',
			date: '',
			timing: '',
			guesttype: '',
		});
	}

	editRow(index: number, functionData: any): void {
		const row = this.collection[index];
		this.rows.push({
			occasions: row.occasions,
			date: { year: row.date.year, month: row.date.month, day: row.date.day },
			time: { hour: row.time.hour, minute: row.time.minute },
			guesttype: row.guesttype,
			value: row
		});
		this.collection.splice(index, 1);
	}

	deleteRow(index: number) {
		this.collection.splice(index, 1);
	}

	hoveredDate: NgbDate | null = null;

	fromDate: NgbDate | null;
	toDate: NgbDate | null;

	constructor(private calendar: NgbCalendar, public formatter: NgbDateParserFormatter) {
		this.fromDate = calendar.getToday();
		this.toDate = calendar.getNext(calendar.getToday(), 'd', 10);
		this.model = this.calendar.getToday();
	}

	onDateSelection(date: NgbDate) {
		if (!this.fromDate && !this.toDate) {
			this.fromDate = date;
		} else if (this.fromDate && !this.toDate && date && date.after(this.fromDate)) {
			this.toDate = date;
		} else {
			this.toDate = null;
			this.fromDate = date;
		}
	}

	isHovered(date: NgbDate) {
		return (
			this.fromDate &&
			!this.toDate &&
			this.hoveredDate &&
			date.after(this.fromDate) &&
			date.before(this.hoveredDate)
		);
	}

	isInside(date: NgbDate) {
		return this.toDate && date.after(this.fromDate) && date.before(this.toDate);
	}

	isRange(date: NgbDate) {
		return (
			date.equals(this.fromDate) ||
			(this.toDate && date.equals(this.toDate)) ||
			this.isInside(date) ||
			this.isHovered(date)
		);
	}

	validateInput(currentValue: NgbDate | null, input: string): NgbDate | null {
		const parsed = this.formatter.parse(input);
		return parsed && this.calendar.isValid(NgbDate.from(parsed)) ? NgbDate.from(parsed) : currentValue;
	}

	saveData(row: any) {
		this.collection.push({
			occasions: row.occasions,
			date: { year: row.date.year, month: row.date.month, day: row.date.day },
			time: { hour: row.time.hour, minute: row.time.minute },
			guesttype: row.guesttype
		});
		this.rows = this.rows.filter(r => r !== row);
	}
}


Normal UI stuffs you gotta go through the internet as there is a lot and no one can learn everything you gotta get experience by practicing more and more then only you will understand how things work 