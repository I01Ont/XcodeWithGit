#Xcode使用Git源码版本控制

相信你了解了Git后，你会爱不释手的，Git是一个版本控制系统，现在无论是在多人共同开发或个人开发时，代码变得越来越复杂，使用Git可以更方便在代码出现问题后，能快速恢复到原来的版本。但这里就不会过多的介绍Git的内容了。

建议是动手去实现这个实例项目，如此一来，更能体会git的妙处。

下面的作为示范的Xcode版本为：7.0.1。

####创建一个Git源 (Creating a Git repository)

打开Xcode，创建一个新的工程，在左边的iOS区里选择“Application”，在应用模板页面选择“Single View Application”，点击“Next”。

![Single View](http://github.com/I01Ont/XcodeWithGit/master/Images/Single_View_Application2.png)

在项目名中输入GitDemo，并在Devices菜单选择iPhone，然后点击“Next”。

![Single View](https://github.com/I01Ont/XcodeWithGit.git/Images/Name_And_Devices.png)

选择保存工程（Project）的目录，再看到窗口底部的Source Control：Create git repository on (My Mac)，在默认下，此选项已经勾选了，如果你不想使用Git，可以取消勾选。现在要的就是勾选它并点击“Create”按钮。

![Project Save](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Porject_Save.png)

当你创建完项目后，找到项目存储的目录，在目录中有一个.git的子目录，是Xcode为了存储git源的相关数据而自动创建的。

![Show .Git](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Show_.Git.png)

如果你看不到.git目录，那是文件隐藏了，做法是打开Terminal，并输入以下命令：

系统是 OS X Mavericks 10.9及以上：

`defaults write com.apple.finder AppleShowAllFiles TRUE`
 
系统是以前的 OS X 版本：

`defaults write com.apple.Finder AppleShowAllFiles TRUE`

然后是要重启Finder，则输入：

`killall Finder`

就能看到本项目的本地git源的存储位置了，实际上，在你选上了相应的选项时，这个目录就会被创建，在你创建新应用时，.git子目录也会被创建。

在使用Xcode创建一个git源是很轻松的事，然而，如果你项目已经创建了，但并未创建git源，现在想添加这个功能，如何做？其实是可以在任何时候为你的项目创建.git源，但尽早创建最好，那么，现在就说说如何做：

首先，新创建一个工程，把工程名命名为NoGit，在选择存储目录时，取消那个创建git源的选项，然后点击“Create”按钮，一个新的工程（没有创建git源）就完成了。

打开Terminal窗口，输入以下命令：

`cd /Users/your username/Desktop/NoGit`

其中，“your username”应换成你的Mac的用户名，紧接着输入：

`git init`

“git init”会初始化出一个空的git源，当你在Finder里查看或输入命令ls时，可以看到.git子目录已经被创建，接着输入：

`git add .`

如此一来，当前目录所有的内容都会添加到源中，再输入：

`git commit -m 'Initial commit'`

接着会出现一个本地git源的目录列表，如图：

![Git_1](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Git_1.png)

OK，现在git源已经创建了，但是当你在Xcode，打开Source Control菜单时，你会看到相关的选项都被禁用了。如图：

![Source Control_No](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Source_Control_No1.png)

原因是在使用Terminal创建git源时，Xcode并没接收到通知，关闭Xcode，重启后，在NoGit项目中，再次打开Source Control菜单时，相关的选项就能使用了，这如同在创建项目时勾选了Source Control：Create git repository on (My Mac)一样。

![Source Control_Can](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Source_Control_Can1.png)

明白上面的命令后，即使你在创建一个Project时没有添加git源，你也可以在任何时候再添加也行。

***

####提交更改（Committing Changes）

提交更改指的是存储一个包含所有更改和改动的新版本，通常，在一个项目完成第一个版本时，就可以提交更改。当然，至于什么时候并没有强制性的要求。

在默认情况下，Xcode在项目创建成功时自动会提交一次更改，目的是为了保存项目的初始状态。

打开Source Control菜单，点击History，就可以看到初次提交更改的记录，所有提交更改的记录都会在这里显示。

接下来，我们在ViewController.m文件中，添加以下属性声明：

    @interface ViewController ()
    
    @property (nonatomic) int sum;
    
    @end
    


接着，修改viewDidLoad方法：

	- (void)viewDidLoad {
	
    	[super viewDidLoad];
    
    	// Do any additional setup after loading the view, typically from a nib.
    
    	int a = 7, b = 8;
    
    	self.sum = a + b;
    
    	NSLog(@"The sum is: %d", self.sum);
    
    }


查看一下Project Navigator面板，会看到在ViewController.m文件的右侧，有个M字母，如图：

![Change Mark](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Change_Mark.png)

说明了ViewController.m文件已经被修改，出现这个M字母是在提醒你有未提交的更改。

那如何提交更改？这很简单，打开Source Control > Commit菜单，就会出现下图：

![ViewControl.m Commit](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/ViewControl.m_Commit1.png)

在最左边区域，只显示ViewControl.m文件被修改了，在文件的左边有个选择框，在默认状态下是被选中，取消勾选后，这文件的更改就不会被提交。

在窗口的中间区域，显示有两个预览窗口，在左边的是文件的当前版本，在右边的是文件上一次提交更改时的版本。

其中，在左边窗口显示蓝色区域的便是要提交的更改内容，清晰明了地知道哪些地方有所修改。

在两个窗口之间有一个有数字显示的小标签，这个数字表示了各项更改，在数字旁边有一个小勾，表示此更改将会被提交，当你点击右边的小箭头时会给出一个选择菜单，明确告诉你可以选择不提交或者忽略此更改。

![Not Commit Or Discard Change](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Not_Commit_Or_Discard_Change.png)

如果你选择“Don't Commit”选项，小勾会由一个停止标志所取代，此项更改便不会被保存到源中。

![Discard Commit](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Discard_Commit.png)

如果你选择“Discard Change”选项，会给出一个确认窗口，提示你所做的更改将会被恢复，并且不能撤消这个操作。

![Dischange](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Dischange.png)


如果你点击了"Revert"按钮，所选中的区域的更改将会完全消失，如同未曾出现。

其中，你细心看看，你所做的所有修改都会被Xcode视作更改。即使是一个空行，而空行如同回车，在屏幕上不可见的，因此作为改变也不无道理。

另外，在提交窗口底部的空白区域（如下图），这里可以添加关于此次更改的描述。

![Write something](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Write_something.png)

当要提交时，就点击窗口右下角的“Commit 1 File”按钮（如下图）：

![Commit 1 File](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Commit_1_File.png)

其中，按钮会显示要提交的文件总数，提交成功后，打开Source Control > History,提交的记录会在列表中显示。

![Project history](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Project_history.png)

关闭Project history窗口，看到Project Navigator,会发现那个在ViewController.m右边的M字母不见了。

好的，现在准备新的一次提交，我们要添加一些新文件，创建个新类，按下Command + N组合键，添加一个Objective-C类，让这个类继承NSObject类，取名为TestClass。添加后，看到Project Navigator,你会发现新创建的文件旁边有个A字母标识，说明了这些文件还未曾提交过。打开ViewController.h文件，导入新类：

    #import "TestClass.h"

另外，打开ViewController.m文件，声明一个私有属性：

    @interface ViewController.m ()
    
    @property (nonatomic) int sum;
    
    @property (nonatomic, strong) TestClass *testClass;        
    
    @end
    

看到Project Navigator,有四个文件待提交，打开Source Control > Commit菜单。

![Commit 5 Files](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Commit_5_Files.png)

可以看到要提交的文件一共有五个，除了修改的四个文件外，还有一个项目配置文件，在新类被添加到项目后，Xcode会自动修改这个项目配置文件，当你打开TestClass.h或TestClass.m文件，左边的窗口没有任何显示：


![CommitFiles](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Commit_5_Files1.png)


原因是此文件在之前并没有提交过的记录，因此没有可以比较的版本，在右边只显示“File was added”。

写上描述并提交便可，可到Source Control > Histroy菜单查看提交记录：

![Project history1](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Project_history1.png)

####版本间的比较（Comparing Versions）

在你提交了同一个工程的不同版本后，在各个版本间相比较、寻找修改信息是非常方便的。

如果你要比较同一文件的两个版本，可以会用View > Version Editor > Show Version Editor或者点击工具栏的Show the version editor > Comparison.

![Comparsion](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Comparsion.png)

这样编辑器区域会分为两部份，最初是显示同样的内容，点击了（下图）小时钟图标，便可比较不同版本的更改内容。

![Compare](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Compare.png)


通常，在左边会显示当前版本内容，而右边会显示之前的版本内容，其中在蓝色高亮的区域显示的是被更改的代码。

![Compareddifferent](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Compare_different.png)

请注意在两个编辑器中间，也有在提交更改窗口时看到的小标签，点击了向下的按钮会给出忽略更改的选项，如果你点击了忽略更改，Xcode会给你提示并征求你的同意，如果你选择同意忽略，这些被忽略的代码将会永远不复存在，因此注意不要忽略。

当然，除了上述的方法，另外还有一个回到之前版本的文法，在两个编辑器下面的工具栏，下图箭头指出的图标：

![Dischange1](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Dischange1.png)

当你点击它后，两个面板间的纵列内容便会改变，变成了之前更改的时间戳，但并不是所有的更改都代表实际提交。代表之前版本的圆角矩形的数量取决于提交更改的次数。

![time](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Time.png)

除了文件的不同版本的比较之外，Xcode也可以让你找到文件的更改提交者，以及是谁修改了哪些代码，在多人开发的团队中，这是非常有用的。打开View Editor > Show Blame View菜单或是在工具栏的Show the version editor按钮上选择Blame选项。

![Compare5](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Compare5.png)

显示不同的提交和作者，还有其它的提交信息都显示在右边的面板中。

![compare4](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Compare4.png)

在下图中，点击（提示1），跳到特定提交更改的文件，点击（提示2），会跳转到比较窗口。

![Compare8](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Compare8.png)

其中，有一个日志视图（Log View）。可以打开View > Version Editor > Show Log View菜单查看。

####分支（Branches）

当你的工程的版本将要发布或已经发布了，你想要添加一些新的特性进去，但又要预防新添加的代码会令整个项目出错，这时你就要用到分支了，在默认状态下git中都会有一个分支，称为master。在项目创建时xcode自动提交的第一次更改就是在这个分支里，但是，如果整个项目只有master这个分支，这是一个不好的习惯，应当避免并使用分支，这样能避免不少麻烦。

另外，关于分支的注意事项：

* 提交的最终产品一定是项目的master分支项目。

* 所有的分支（除了master分支）中实现的代码、功能最后都要合并到master分支。

在你添加一个新的分支时，是以当前项目状态为起点的，所有的修改、更改也只会在当前分支体现。

打开Source Control > GitDemo-master > New Brance...这个菜单：

![AnotherBranch](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/AnotherBranch.png)

命名随意，点击“Create”按钮，新的的分支就会被创建了，而当前的代码也会被复制到新的分支里。

打开Source Control菜单，可以看到活动分支是哪个了：

![GitDemo_Anotherbranch](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/GitDemo_Anotherbranch.png)

打开类文件，在私有属性区添加下列方法声明：

    @interface ViewController ()
    
    ...
    
    - (void)sayHello;
    
    @end
    

接着实现：

    - (void)sayHello {
    	NSLog(@"Hello");
    }
    
    
在viewDidLoad中调用它：

    - (void)viewDidLoad {
    
    ...
    
    	[self sayHello];
    }
    
    
 现在，将AnotherBranch分支的更改提交了，打开View > Version Editor >Show Version Editor菜单，在右边编辑面板下面的工具栏，可以看到被选中的分支是AnotherBranch,点击便可看到当前分支和master分支同时显示，在master分支里选择任意版本，Xcode都会高亮地显示这二者的区别，如此便能观察到代码间的更改了。
 
 ![master_AnotherBranch](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/master_compare_AnotherBranch.png)
 
 另外，要切换到另一个分支，可以打开Source Control > GitDemo - AnotherBranch > Switch to Branch...菜单。
 
 ![Switch_Branch](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Switch_to _Branch.png)
 
 在下图中可以选择想跳转的分支，现在选择master。
 
 ![Switch_master](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Switch_Master.png)
 
 当master分支成为当前活动分支时，可以观察到在AnotherBranch的更改并没有在master中体现。
 
####合并分支（Merging Branches）

在分支中进行开发是一种好习惯，但要想把代码整合到现在的发行版本里，那就要进行分支合并了。

首先，确保当前活动分支是master分支，如果不是就要换回master分支，现在创建一个新的分支，打开Source Control > GitDemo - master > New Branch...菜单。命名为LastBranch。

然后到ViewController.m文件中，创建一个私有方法，现在声明它：

    @interface ViewController ()
    
    ...
    
    - (void)sayByeBye;
    
    @end
    
    
接着实现：

    - (void)sayByeBye {
    
    	NSLog("ByeBye");
    	
    }
   
   
在viewDidLoad方法中调用：

    - (void)viewDidLoad {
    
    ...
    
    [self sayByeBye];
    
    }
   
   
在合并前，要先把更改提交了，点击Source Control > Commit菜单提交。关于把两个不同的分支合并，有两种选择：


* 从分支中合并：你所选择分支的相关的任何改变都将会被合并到当前活动分支中。

* 合并到分支：当前活动分支的任何改变都将会被合并到所选择的分支中。

上述的两种情况都能在Source Control > GitDemo菜单中实现，但值得注意的是，当活动分支是master分支时，第二个选择是不可选的。

![Merge2](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Merge2.png)

现在开始合并示范：确保当前活动分支是master分支，打开Source Control > GitDemo - master > Merge From Branch...菜单，选择AnotherBranch,然后点击Merge按钮。

![Merge_AnotherBranch](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Merge_AnotherBranch.png)

紧接着会有一个比较窗口，可以看到合并之后代码的更改，觉得可以了就点击Merge按钮。

![Merge1](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Merge1.png)

等会就OK了，完成后AnotherBranch里的内容都合并到master分支中。然后，用同样的方法合并LastBranch。值得注意的是，你要先提交完更改，Xcode才会允许合并，在比较窗口区域的中间，你能看到有一个红色区域显示合并后的更改，这表明分支里的代码将会替换原来活动分支里的代码，

![Merge_replace](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Merge_replace.png)

在编辑面板底部的一排的四个小按钮，选择第一个，会把master分支的代码放在上面，而另一个分支的代码则会在其后面。另外三个小按钮，可以自行点击看看，这里不做介绍了。

![](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Merge_replace1.png)

细心地查看要更改的代码，可以后就点击窗口右下角的Merge按钮，这样就能合并了。

####忽略更改（Discarding Changes）

在当你在开发过程中发现了在大问题时，要从让开发回到上一个稳定的版本重新开始时，忽略更改功能就显示得很重要了，但值得注意的是，放弃更改操作，一经确定后是无法撤消的，因此要慎重对待。

前面我们在讨论版本间的比较时，学会如何忽略其中部分代码更改的方法，接着，要学的是，如何忽略自从上次提交更改的所有更改。

打开ViewController.h,添加一个公共方法声明：

    @interface ViewController: UIViewController
    
    - (void)aVeryCoolMethod;
    
    @end
    
    
接着在ViewController.m添加一个上面方法的实现:

    - (void)aVeryCoolMethod {
    
    	NSLog(@"Don't discard me,place.");
    	
    }
    
    
看到Project Navigator,刚才更改的文件旁边有个M字母标识，当忽略所有更改后，看看是否会回到更改之初的状态。当然，你也可以选择忽略单个文件或多个文件的更改，首先要选定要忽略更改的文件，然后打开Source Control便能看到Discard Change的相关选项，
这个可以自己实践下。当你确定点击了Discard All Changes后，会给出一个提示框（如下图），让你确认是否要删除。这能防止误删代码。

![Discard_All_Changes](http://github.com/I01Ont/XcodeWithGit/raw/master/Images/Discard_All_Changes.png)

好了，可以看出在Xcode中使用git管理是多么的好用，希望本教程对你有所帮助，若有错误之处，还望你提出和教导。



###### 参考：[在Xcode中使用Git进行源码版本控制 - IOS - 伯乐在线](http://ios.jobbole.com/83883/)


