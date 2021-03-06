# ICommand

View에서 발생하는 이벤트를 ViewModel에서 처리하기위해서는 <br/>
ICommand를 사용해야한다.


```
xmlns:xe="clr-namespace:XamEffects;assembly=XamEffects"

<Grid  xe:Commands.Tap="{Binding LogoutTap}">
<Grid  xe:Commands.Tap="{Binding UserDeleteTap}">
```
위와같이 Grid 를 Tap하였을때의 이벤트를 처리하기위해서는 위와같이 Binding를 통해 전달하게되고<br/>
해당 이벤트 Binding는 ICommand를 통해 ViewModel에서 처리한다.

```
public Logout_And_DeletePage()
{
	InitializeComponent();
	BindingContext = new Logout_And_DeleteVM();
}
```
Binding 가 필요한 ViewClass에서는 BindingContext에 ViewModel 을 지정하고

```
class Logout_And_DeleteVM 
{
	public ICommand LogoutTap { get; set; }
	public ICommand UserDeleteTap { get; set; }
	public Logout_And_DeleteVM()
	{
		LogoutTap     = new Command(async () => await Logout_Tapped());
		UserDeleteTap = new Command(async () => await UserDelete_Tapped());
	}
}
private async Task Logout_Tapped(){//로그아웃}
private async Task UserDelete_Tapped(){//계정 제거}
```
ViewModel에서는 View에서 처리하려하는 이벤트 Binding 이름 을 ICommand로 만들어서 처리한다.

## CommandParameter
Command 를 통해 이벤트를 처리하려해도 View의 특정한 데이터를 필요로하는경우가 생긴다.
이때 Command에 파라미터를 넘길수있다.
```
<Entry x:Name="userCnt" VerticalOptions="Center"/>
<Button Command="{Binding BtnCommand}"
		CommandParameter="{Binding Source={x:Reference userCnt},Path=Text}"
		Text="{Binding DIsplayCount}"
		/>
```
위의 코드를 보면 CommandParameter 에 자신위의 control인 Entry의 Text를 파리미터를 넘기는것을 볼수있다.

```
public ICommand BtnCommand { get; }
int count = 0;
public MainVM()
{
	BtnCommand = new Command<string>((cnt) =>BtnCount(cnt));
}

private void BtnCount(string i)
{
	if(int.TryParse(i,out int cnt))
	{
		count += cnt;
	}

}
```
ICommand 에서 해당 파라미터를 전달받아 처리하는 모습이다.<br/>

## Command의 Event처리

```
public ICommand IListViewTapCommand { get; }


public ViewModel()
{
	IListViewTapCommand = new Command<object>((e) => ListViewTep(e));
} 
```

ICommand에서 선언한다.
생성자에서 Command의 CommandParameter를 받아온다.

```
private void ListViewTep(object _commandParameter)
{
	var sender = _commandParameter as Xamarin.Forms.ListView;
	var args = _commandParameter as Xamarin.Forms.ItemTappedEventArgs;
	var tapOb = Args.Item;
	var tapIndex = Args.ItemIndex;
} 
```

CommandParameter에는 Sender와 Args가 같이 담겨있기 때문에 분리를 해준후
Args만 따로 활용하면 된다.


