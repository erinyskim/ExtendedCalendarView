ExtendedCalendarView
====================

This project is meant for people who want to display a calendar view and show that there are events on certain days.
What I did was pull out the calendar view from Google's Calendar application found here 

https://github.com/android/platform_packages_apps_calendar.

Implementation is not the easiest though as there is a lot to it but I hope to filter out stuff that is not needed to try to make it simpler

A quick run though of how to use it:

in your activity before you set the content view you need to create a Calendar Controller

    public class MainActivity extends Activity implements EventHandler{
    
	private CalendarController mController;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);

		mController = CalendarController.getInstance(this);
		setContentView(R.layout.cal_layout);
        mController.registerEventHandler(R.id.cal_frame, (EventHandler) monthFrag);
        
        mController.registerFirstEventHandler(0, this);
	}
	
The put in the MonthByWeekFragment to your view

    FragmentTransaction ft = getFragmentManager().beginTransaction();
				
	monthFrag = new MonthByWeekFragment(System.currentTimeMillis(), false);
        ft.replace(R.id.cal_frame, monthFrag).commit();
        
You also need to override handleEvent, this gets called when you click on a day on the calendar and gets called when you click on an event in the day view

    @Override
	public void handleEvent(EventInfo event) {
		if (event.eventType == EventType.GO_TO) {
		// day selected on calendsr, start DayFragment to display the day that was clicked
			this.event = event;
			dayView = true;
				FragmentTransaction ft = getFragmentManager().beginTransaction();
				dayFrag = new DayFragment(event.startTime.toMillis(true),1);
				ft.replace(R.id.cal_frame, dayFrag).addToBackStack(null).commit();
		}if(event.eventType == EventType.VIEW_EVENT){
			//TODO do something when an event is clicked	
		}
		
	}
	
	
Calendar Content Provider
=========================

To display events on the calendar I have build a content provider that the calendar uses which replicates how it is done in googles calendar app.
