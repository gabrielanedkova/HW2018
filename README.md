# HW2018

#include <iostream>
#include <fstream>
#include "Vehicle.h"

using namespace std;


int main() {

	Vehicle v;
	cout << endl;
	Vehicle b = v;
	char n[] = "new";
	v.setNumber(n);
	v.setYear(2000);
	cout << (v<b);
	system("pause");
}

////////////////////////////////
#pragma once
#include<fstream>
class Vehicle
{

public:
	Vehicle();
	Vehicle(const Vehicle& source);
	Vehicle& operator= (const Vehicle& source);
	~Vehicle();

	friend bool operator< (const Vehicle& left, const Vehicle& right);
	char* getNumber();
	void setNumber(char* number);
	void setYear(int year);


private:
	char* number;
	int year;
	char* model;
	char* ownerName;

	void copyStr(char*& copy, char* original);

};

//////////////////////
#include "Vehicle.h"
#include <iostream>


Vehicle::Vehicle()
{
	char fileName[]= "gabi.txt";
	number = new char[15];
	model = new char[30];
	ownerName = new char[30];
	std::ifstream i(fileName);
	i >> number >> year >> model >> ownerName;

	std::cout <<  number << " " << year << " " << model << " " << ownerName;
}

Vehicle::Vehicle(const Vehicle & source)
{
	copyStr(number, source.number);
	copyStr(model, source.model);
	copyStr(ownerName, source.ownerName);
	this->year = source.year;
}

Vehicle & Vehicle::operator=(const Vehicle & source)
{
	if (&source != this) 
	{
		delete[] number;
		delete[] model;
		delete[] ownerName;
		copyStr(number, source.number);
		copyStr(model, source.model);
		copyStr(ownerName, source.ownerName);
		this->year = source.year;
	}
	return *this;
}



Vehicle::~Vehicle()
{
	std::ofstream o("new.txt", std::ios::app);
	o << number << " " << year << " " << model << " " << ownerName << '\n';
	o.close();
	delete[] number;
	delete[] model;
	delete[] ownerName;
}

bool operator<(const Vehicle & left, const Vehicle & right)
{
	return left.year < right.year;
}

char * Vehicle::getNumber()
{
	return number;
}

void Vehicle::setNumber(char* number)
{
	delete[] this->number;
	this->number = new char[15];
	copyStr(this->number, number);
}

void Vehicle::setYear(int year)
{
	this->year = year;
}

void Vehicle::copyStr(char *& copy, char * original)
{
	if (original != nullptr)
	{
		delete[] copy;
		int len = strlen(original) + 1;
		copy = new char[len];
		for (int i = 0; i < len; ++i)
		{
			copy[i] = original[i];
		}
	}

}

