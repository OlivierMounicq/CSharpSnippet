


```cs
internal class NumericFieldChecker<T> : IErrorChecker
where T : struct
{
  public IEnumerable<Error> CheckValue(object value, int rowIndex, object additionnalInfo = null, string msg = null)
  {
    var cultureInfo = value.ToString().Contains(".") ? "en-US" : "fr-FR";

    Type[] argTypes = { typeof(string), typeof(NumberStyles), typeof(CultureInfo), typeof(T).MakeByRefType() };
		
    var res = (bool)typeof(T).GetMethod("TryParse", argTypes).Invoke(null, new object[] { value as string, NumberStyles.Any, new CultureInfo(cultureInfo), null });

    if (!res)
      yield return new Error(rowIndex, string.Format("The field {0} with the value {1} is not a numeric", msg, value), ErrorType.NotNumeric);
    }
}
```
