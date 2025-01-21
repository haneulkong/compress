import streamlit as st
from PyPDF2 import PdfReader, PdfWriter
from io import BytesIO

def compress_pdf(input_pdf, output_quality="low"):
    """PDF 압축 및 변환"""
    # PyPDF2로 PDF 읽기
    pdf_reader = PdfReader(input_pdf)
    pdf_writer = PdfWriter()

    # 페이지 순회하며 압축 적용
    for page in pdf_reader.pages:
        # 이미지가 포함된 경우 압축 처리
        page.compress_content_streams()  # PyPDF2의 내장 압축 기능
        pdf_writer.add_page(page)

    # 압축된 PDF 저장
    output = BytesIO()
    pdf_writer.write(output)
    output.seek(0)
    return output

# Streamlit App
st.title("PDF 압축 및 변환 도구")
st.write("PDF 파일을 업로드하고 10MB 이하로 압축하세요!")

uploaded_file = st.file_uploader("PDF 파일을 업로드하세요", type=["pdf"])

if uploaded_file is not None:
    st.write("파일 업로드 성공!")
    original_size = len(uploaded_file.getvalue()) / 1024 / 1024
    st.write(f"원본 파일 크기: {original_size:.2f}MB")
    
    if original_size > 10:  # 파일 크기 제한
        st.write("압축 중...")
        compressed_pdf = compress_pdf(uploaded_file)
        compressed_size = len(compressed_pdf.getvalue()) / 1024 / 1024
        st.write(f"압축된 파일 크기: {compressed_size:.2f}MB")
        
        if compressed_size > 10:
            st.warning("압축 후에도 파일 크기가 10MB를 초과합니다. 추가 압축이 필요할 수 있습니다.")
        else:
            st.success("압축 완료! 아래에서 파일을 다운로드하세요.")
        
        # 압축된 파일 다운로드 버튼
        st.download_button(
            label="압축된 PDF 다운로드",
            data=compressed_pdf,
            file_name="compressed_file.pdf",
            mime="application/pdf"
        )
    else:
        st.success("업로드된 파일이 이미 10MB 이하입니다. 추가 압축이 필요하지 않습니다.")
