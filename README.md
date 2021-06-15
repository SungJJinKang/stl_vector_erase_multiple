# stl_vector_erase_multiple
std::vector.erase_multiple

If you want to erase multiple items in std::vector, How do you do that?            
Like this?     

```c++
int reallocationCount = 0;
class A
{
  private:
  ~~ // many datas
  
public:

	A()
	{

	}

	A(const A& a)
	{
		~~ // copy jobs
		reallocationCount++;
	}

	A(A&& a) noexcept
	{
		~~ // move jobs
		reallocationCount++;
	}

	A& operator=(const A& a)
	{
		~~ // copy jobs
		reallocationCount++;
		return *this;
	}

	A& operator=(A&& a) noexcept
	{
		~~ // move jobs
		reallocationCount++;
		return *this;
	}
};

int main()
{
	std::vector<A> aVector(100);
	aVector.erase(aVector.begin());
	aVector.erase(aVector.begin() + 5);
	aVector.erase(aVector.begin() + 10);
	std::cout << "reallocation count : " << reallocationCount << std::endl;
}

output :
reallocation count : 279
```

If you do like this, What happen internally in std::vector.       
```
// aVector.erase(aVector.begin());
second item is moved to first position
third item is moved to second position
4th item is moved to third position
.
.

// aVector.erase(aVector.begin() + 5);
6th item is moved to 5th position
7th item is moved to 6th position
8th item is moved to 7th position
.
.

// aVector.erase(aVector.begin() + 10);
11th item is moved to 10th position
12th item is moved to 11th position
13th item is moved to 12th position
.
.
```

Move Assignment opeartor is called 279 times!!!!     
Everytime erase a item, Items after that item is moved a position.       
If you have not well designed move assignment opeartor, This reallocation will be disaster.         


This repository is made for solving this problem.     

## How to use
```c++
std::vector<A> aVector(100);
aVector.erase(aVector.begin(), aVector.begin() + 5, aVector.begin() + 10);  // Move Assignment opeartor is called JUST 99 times!!!!  
```
