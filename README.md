package com.example.demo;
import java.util.ArrayList;
import java.util.List;

import com.vaadin.annotations.Theme;
import com.vaadin.annotations.Title;
import com.vaadin.data.TreeData;
import com.vaadin.data.provider.TreeDataProvider;
import com.vaadin.server.VaadinRequest;
import com.vaadin.shared.ui.grid.HeightMode;
import com.vaadin.spring.annotation.SpringUI;
import com.vaadin.ui.Button;
import com.vaadin.ui.CheckBox;
import com.vaadin.ui.CheckBoxGroup;
import com.vaadin.ui.ComboBox;
import com.vaadin.ui.Grid;
import com.vaadin.ui.Label;
import com.vaadin.ui.Notification;
import com.vaadin.ui.RadioButtonGroup;
import com.vaadin.ui.TextField;
import com.vaadin.ui.Tree;
import com.vaadin.ui.Tree.TreeContextClickEvent;
import com.vaadin.ui.UI;
import com.vaadin.ui.VerticalLayout;
import com.vaadin.ui.Grid.SelectionMode;
import com.vaadin.ui.renderers.ButtonRenderer;

@SpringUI(path="/ui")
@Title("Welcome To Vaadin")
@Theme("valo")
public class MainView  extends UI{

	@SuppressWarnings({ "deprecation", "rawtypes", "unchecked" })
	protected void init(VaadinRequest request) {

		VerticalLayout verticalLayout = new VerticalLayout();

		// Lable
		Label lable = new Label("Welcome to New Application");
		verticalLayout.addComponent(lable);

		// TextFields
		TextField textfields = new TextField();
		textfields.setValue("Name : ");
		textfields.setMaxLength(20);



		//Display the length on the lable 

		Label lable1 = new Label();
		lable1.setValue(textfields.getValue()+ " " + textfields.getMaxLength());	

		// if you want to know the how many value are typed in the text use this valueChangeListner

		textfields.addValueChangeListener(event ->{

			int lengthCounter = event.getValue().length();
			lable1.setValue(lengthCounter + " " + textfields.getMaxLength());
		});

		// Button 

		Button button  = new Button("ClickMe");
		button.addClickListener(eve->{
			lable1.setValue(textfields.getValue());
			Notification.show("Button has clicked");
		});

		//CheckBox 
		CheckBox customCheckBox = new CheckBox("CustomCheckBox");
		CheckBox normalCheckBox = new CheckBox("NormalCheckBox");

		customCheckBox.addValueChangeListener(event->{

			Button button1 = new Button("Custom Button");
			button1.setStyleName("This is the Custom Button");
			verticalLayout.addComponent(button1);

		});

		normalCheckBox.addValueChangeListener(event->{
			Button button2 = new Button("Normal Button");
			button2.setStyleName("This is the Normal Button");
			verticalLayout.addComponent(button2);
		});

		// RadioButtonGroup SingleSelection

		RadioButtonGroup<String> singleSelection = new RadioButtonGroup<>("E-shop");
		singleSelection.setItems("shoes" ,"jense","shirts","pants");

		//MultiSelection
		CheckBoxGroup<String> multiSelecton = new CheckBoxGroup<>("E-shoping");
		multiSelecton.setItems("Lenovo","Del","Apple","Micromax");

		// if you want disable the  the item use this code

		RadioButtonGroup<String> disableGroup = new RadioButtonGroup<>();
		disableGroup.setItems("shoes" ,"jense","shirts","pants");
		disableGroup.setItemEnabledProvider(item->!"jense".equals(item));

		// ComboBox :-

		ComboBox<String> combo = new ComboBox<>();
		List<String> list = new ArrayList<>();
		list.add("Rock");
		list.add("John");
		list.add("Peter");
		list.add("Emily");
		list.add("nayan");
		combo.setItems(list);

		// add the dynamically value in the combobox

		combo.setNewItemHandler(inputValue->{

			String boxer = inputValue;
			list.add(boxer);
			combo.setItems(list);
		});

		// Grid Tables

		List<Person> person = new ArrayList<>();
		person.add(new Person(10, "Emily", "Jack", "Us", true));
		person.add(new Person(45, "Irfan", "patan", "pakistan", false));
		person.add(new Person(70, "Subash", "Raj", "India", false));
		person.add(new Person(15, "varsha", "josh", "India", true));
		person.add(new Person(13, "mick", "Jack", "Us", true));
		person.add(new Person(18, "Priya", "Datta", "India", true));

		Grid<Person> grid = new Grid<Person>();

		grid.addColumn(Person::getFirstName).setCaption("First_Name");
		grid.addColumn(Person::getLastName).setCaption("Last_Name");
		grid.addColumn(Person::getLocation).setCaption("Location");
		grid.addColumn(Person::getAge).setCaption("Age");
		grid.addColumn(Person::isActive).setCaption("IsActive");

		// Delete Item 

		grid.setSelectionMode(SelectionMode.MULTI);
		grid.addColumn(events ->"Delete",
				new ButtonRenderer(clickEvent->{
					person.remove(clickEvent.getItem());
					grid.setItems(person);
				}));	

		grid.setHeightMode(HeightMode.ROW);
		grid.setHeightByRows(person.size());
		grid.setItems(person);

		// Tree
		Tree <String>tree = new Tree("Programing technologies");
		TreeData<String> treeData = new TreeData<>();

		treeData.addItem(null,"BackEnd");
		treeData.addItem(null,"FrontEnd");

		treeData.addItem("BackEnd","java");
		treeData.addItem("BackEnd","Mysql");
		treeData.addItem("BackEnd","Rest");
		treeData.addItem("BackEnd","Maven");

		treeData.addItem("FrontEnd","css");
		treeData.addItem("FrontEnd","Html");
		treeData.addItem("FrontEnd","javascript");

		tree.setDataProvider(new TreeDataProvider<>(treeData));
		tree.expand("BackEnd");
		tree.expand("FrontEnd");
		tree.addItemClickListener(event->Notification.show("Clicked",Notification.TYPE_HUMANIZED_MESSAGE));
		tree.addContextClickListener(event->Notification.show(((TreeContextClickEvent<String>)event).getItem()+ "Clicked"));






		verticalLayout.addComponent(tree);
		verticalLayout.addComponent(grid);
		verticalLayout.addComponent(combo);
		verticalLayout.addComponent(disableGroup);
		verticalLayout.addComponent(multiSelecton);
		verticalLayout.addComponent(singleSelection);
		verticalLayout.addComponent(button);
		verticalLayout.addComponent(normalCheckBox);
		verticalLayout.addComponent(customCheckBox);
		verticalLayout.addComponent(lable1);
		verticalLayout.addComponent(textfields);

		setContent(verticalLayout);;

	}

}
