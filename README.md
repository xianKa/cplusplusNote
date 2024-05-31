TestGit --
XiankaGao
@Composable
fun TextFieldWithOverflowIndicator() {
    var text by remember { mutableStateOf("") }
    var isOverflow by remember { mutableStateOf(false) }

    val textFieldWidth = remember { mutableStateOf(0) }
    val textMeasurer = rememberTextMeasurer()

    val textStyle = TextStyle(fontSize = 16.sp)

    LaunchedEffect(text) {
        val textLayoutResult = textMeasurer.measure(
            text = AnnotatedString(text),
            style = textStyle,
            constraints = Constraints(maxWidth = textFieldWidth.value)
        )
        isOverflow = textLayoutResult.didOverflowWidth
    }

    Column(
        modifier = Modifier
            .padding(16.dp)
            .fillMaxWidth()
    ) {
        BasicTextField(
            value = text,
            onValueChange = { newText ->
                text = newText
            },
            textStyle = textStyle,
            modifier = Modifier
                .onSizeChanged { size ->
                    textFieldWidth.value = size.width
                }
                .background(Color.White)
                .border(1.dp, Color.Gray)
                .padding(8.dp)
                .fillMaxWidth()
        )

        if (isOverflow) {
            Text(
                text = "Text overflow!",
                color = Color.Red,
                fontSize = 12.sp,
                modifier = Modifier.padding(top = 4.dp)
            )
        }
    }
}
