Pyrolite - Python Remote Objects "light" and Pickle for Java/.NET

  Pyrolite is written by Irmen de Jong (irmen@razorvine.net).
  This software is distributed under the terms written in the file `LICENSE`.


Contents:
    1. INTRODUCTION
    2. THE LIBRARY
    3. TYPE MAPPINGS
    4. EXCEPTIONS
    5. SECURITY WARNING
    6. RECOMMENDED DEPENDENCY: SERPENT SERIALIZER
    7. DOWNLOAD COMPILED BINARIES


1. INTRODUCTION
---------------------

This library allows to pickle and unpickle python streams.
It based on Pyrolite project, but was mavenized and only java and pickle left.


2. THE LIBRARY
---------------------

The library is a feature complete implementation of Python's pickle protocol,
including memoization. It is fully compatible with pickles from Python 2.x and
Python 3.x, and you can use it idependently from the rest of the library, to
read and write Python pickle structures.

3. TYPE MAPPINGS
---------------------

Pyrolite does the following type mappings:

PYTHON    ---->     JAVA
------              ----
None                null
bool                boolean
int                 int
long                long or BigInteger  (depending on size)
string              String
unicode             String
complex             net.razorvine.pickle.objects.ComplexNumber
datetime.date       java.util.Calendar
datetime.datetime   java.util.Calendar
datetime.time       java.util.Calendar
datetime.timedelta  net.razorvine.pickle.objects.TimeDelta
float               double   (float isn't used) 
array.array         array of appropriate primitive type (char, int, short, long, float, double)
list                java.util.List<Object>
tuple               Object[]
set                 java.util.Set
dict                java.util.Map
bytes               byte[]
bytearray           byte[]
decimal             BigDecimal    
custom class        Map<String, Object>  (dict with class attributes including its name in "__class__")
Pyro4.core.URI      net.razorvine.pyro.PyroURI
Pyro4.core.Proxy    net.razorvine.pyro.PyroProxy
Pyro4.errors.*      net.razorvine.pyro.PyroException
Pyro4.utils.flame.FlameBuiltin     net.razorvine.pyro.FlameBuiltin 
Pyro4.utils.flame.FlameModule      net.razorvine.pyro.FlameModule 
Pyro4.utils.flame.RemoteInteractiveConsole    net.razorvine.pyro.FlameRemoteConsole 

The unpickler simply returns an Object. Because Java is a statically typed
language you will have to cast that to the appropriate type. Refer to this
table to see what you can expect to receive.
                    

JAVA     ---->      PYTHON
-----               ------
null                None
boolean             bool
byte                int
char                str/unicode (length 1)
String              str/unicode
double              float
float               float
int                 int
short               int
BigDecimal          decimal
BigInteger          long
any array           array if elements are primitive type (else tuple)
Object[]            tuple (cannot contain self-references)
byte[]              bytearray
java.util.Date      datetime.datetime
java.util.Calendar  datetime.datetime
Enum                the enum value as string
java.util.Set       set
Map, Hashtable      dict
Vector, Collection  list
Serializable        treated as a JavaBean, see below.
JavaBean            dict of the bean's public properties + __class__ for the bean's type.
net.razorvine.pyro.PyroURI      Pyro4.core.URI
net.razorvine.pyro.PyroProxy    cannot be pickled.

